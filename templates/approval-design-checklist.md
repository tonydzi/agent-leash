# Approval design checklist: irreversible actions

Use this when wiring human approval into an agent pipeline. Every unchecked box is a hole an attacker or an eager agent will eventually find. Companion doc: [plan-vs-authorize](../docs/plan-vs-authorize.md).

## 1. Enumerate

- [ ] Irreversible/outward action classes are written down (money, outbound to humans, deletion, publishing, credential use, legal commitments)
- [ ] Unlisted action classes default to REQUIRE approval (highest tier), not to "allowed"
- [ ] There is a written "we never automate this" list, however short

## 2. Gate placement

- [ ] The gate is deterministic code, not a model judging its own request
- [ ] The gate sits between the model and the executor; the model has no direct path to the action
- [ ] The executor's credential only covers the gated action class (a payment executor cannot delete repos)

## 3. The approval channel

- [ ] Approval travels out-of-band: a channel separate from the content the agent reads while working
- [ ] Approver identity is allowlisted by account id (display names and phone-book names do not count)
- [ ] The approval is an explicit token bound to a request id, not free text or an emoji reaction
- [ ] Text the agent read in ANY input can never be interpreted as approval (chat content = data, always)
- [ ] The agent cannot approve its own request from any account it controls

## 4. Time and state

- [ ] Approvals expire (define the window; 15 minutes is a sane default for interactive flows)
- [ ] One approval covers ONE request id; retries and batches need their own
- [ ] A denied request stays denied; the agent must not re-ask in a loop (log and move on)

## 5. Human factors

- [ ] The approval request states: what, why, risk, and what happens on "no", in one message the approver can judge from a phone
- [ ] There is a defined path when the approver is unreachable (queue and wait; degrade autonomy; never "proceed by timeout")
- [ ] The approver can also HALT everything with one message (kill switch in the same channel)

## 6. Evidence

- [ ] Every approval/denial lands in an append-only log: request id, approver id, timestamp, outcome
- [ ] The DENY path has been tested on purpose at least once (fire a gated action, refuse it, verify nothing happened)
- [ ] The BYPASS path has been tested on purpose at least once (try to trick the gate with content that looks like approval; verify it fails)

## 7. Scope, fatigue, and the gate's own health

The first six sections are about the gate being right. This one is about the gate still being
there in six weeks. Every item below is a hole we walked into on our own fleet.

- [ ] An approval is bound to the **action class**, not to the container that happened to run it (task, session, thread, worker). Write down what re-approves when the same work is launched a second way
- [ ] Approval history is not a fact about plumbing. If reusing a warm container skips prompts that a fresh one raises, operators will optimise for reuse and broad pre-approval - that is the design teaching them to stop reading
- [ ] The gate's own failure is **loud**. A gate whose `except` returns "allow" is worse than no gate: it fails open, the prompts you removed quietly come back or quietly stop, and only a log knows
- [ ] The gate has a regression test that is run after every change to what it calls. Ours died from a helper that started returning three groups instead of two; the suite had not been run since that change
- [ ] You know which actions your platform hard-asks for **regardless of every lever you set**, and you have designed around them rather than assuming your configuration covers everything
- [ ] Per-machine state (approval caches, worker pools, slot registries) lives per machine. A single synced file showing "the" state shows you one random machine's state

### Measured, 2026-07 to 2026-08, one fleet of six machines

- The platform stored granted tool-approvals **on the task object**, auto-applied to that task's
  future runs. Identical work launched as a *new* task therefore began with an empty approval
  history and stopped on the first prompt, while the same work in a reused task never stopped.
  Nothing about the risk changed between those two runs - only the container.
- One call (creating a scheduled task) raised the dialog with permissive mode, an explicit
  allowlist **and** a pre-tool hook returning "allow" all in place, while its siblings passed
  silently. One unsilenceable action is not a bug to route around quietly; it is a fact your
  design has to state.
- The guard enforcing container reuse crashed on an internal API change and its
  `except Exception -> allow` turned that into a silent pass. The prompts came back to a human
  who had been told they were gone. Fail-open is a legitimate choice; **silent** fail-open is not.
- The registry of reusable containers lived in one synced file. Two machines overwrote each
  other's counts inside an hour ("free 1 / convertible 20" then "20 / 10"), so the card showed a
  random machine's pool with no indication which.

## Common designs that fail this checklist

| Design | Which box it fails |
|---|---|
| "The model asks itself 'is this safe?' before acting" | 2.1: the judge is the suspect |
| "Approval = user says ok in the same chat the agent reads documents in" | 3.1/3.4: injected text can fake it |
| "One 'yes' at the start of the session covers everything after" | 4.1/4.2: stale, unbounded scope |
| "If no answer in 10 minutes, proceed" | 5.2: timeout-as-consent inverts the default |
| "Approvals are in the agent's own memory/log file" | 6.1: the suspect keeps the evidence |
| "Approval is remembered per task/session, so reuse the warm one" | 7.1/7.2: scope follows the container, not the action; teaches reuse instead of reading |
| "The gate catches its own exceptions and allows on error" | 7.3: fail-open that nobody can see is an outage of the safety system |
| "One shared file holds the fleet's approval state" | 7.6: every machine reads someone else's answer |
