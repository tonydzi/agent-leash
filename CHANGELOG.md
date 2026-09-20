# Changelog

## v0.1.3 - 2026-09-20

One new checkbox in the approval checklist, and one number in the README corrected downward.

- **[templates/approval-design-checklist.md](templates/approval-design-checklist.md) §7 - the
  gate has to read the whole set it claims to guard.** This is not fail-open-on-exception, which
  §7 already covered. Nothing errors and no log line goes missing: the gate simply has a smaller
  idea of the damage area than the operation it guards, so it prints a confident "nothing found"
  over the part it never looked at. Measured on our own publication gate: it scanned the diff
  against the last commit, diffs do not list *untracked* files, and new files are exactly what
  publishing adds. Seeded with eight kinds of credential in a new file, it caught none of the
  seven real ones - private key and cloud access key included - and printed "0 lines scanned, 0
  findings". Widening the input turned all eight seeds red first, then green. The failing-designs
  table gained the matching row: *"the scanner checks the diff / the changed rows / the last
  batch"*.
- **The ClawHavoc figure is now the audited count, not a rounded ratio.** The README opened with
  "roughly one in five marketplace skills compromised". The audit it refers to found 341 malicious
  skills among 2,632, and 824 once that marketplace grew past 10,700 - so the README says that
  instead. A security argument that rounds its own evidence invites the reader to round the rest.
- README claims are anchored to the files and dates that back them, and the repo points at the
  system map (SYSTEM.md) it belongs to.

## v0.1.2 - 2026-09-05

Docs only: names the layer above LEASH-8, which the repo described per-agent and never
per-fleet.

- **README: the fleet layer (kill, cap, replay).** Most agent-security tooling guards one
  agent's next action; the failure that costs money starts one floor up, the moment agents run
  on more than one machine. Kill has to mean the fleet, because a stop that needs ssh into six
  boxes at 3am is not a control. Cap has to be set before the run and enforced outside the
  model, because an agent that can be talked into a bigger budget has no budget. Replay has to
  be attributable per agent and per machine, so "what changed last night and who did it" is a
  query rather than an investigation. With a diagram of how the three sit around the fleet.
- The scale claim in the opening paragraph is now stated instead of implied: 6 machines across
  Windows, macOS and Linux, 200+ scheduled agent routines, unattended for months. Re-counted on
  2026-09-05 — 6 nodes in the fleet registry, 248 enabled scheduled tasks on the hub alone, so
  the fleet-wide "200+" is the conservative reading.
- Says plainly that LEASH-8 is the per-agent half and the fleet half is being carried into the
  open piece by piece, rather than implying it is already published.

## v0.1.1 - 2026-08-29

One new section in the approval checklist, drawn from two months of running agents with
delegated authority on a six-machine fleet. It is about the gate still being there in six
weeks, not about the gate being right on day one.

- [templates/approval-design-checklist.md](templates/approval-design-checklist.md) §7 -
  **scope, fatigue, and the gate's own health**: approval bound to the action class rather than
  to the container that ran it; approval history that is a fact about plumbing teaches operators
  to optimise for reuse instead of reading; a gate whose `except` returns "allow" is a silent
  fail-open and only a log knows; per-machine state kept in one synced file shows a random
  machine's answer. Each item is a hole we walked into, with the measurement.
- Three more rows in *common designs that fail this checklist*.

## v0.1.0 - 2026-08-04

Tagged on 2026-08-04. It was written on 2026-07-04 and the note below said "tag planned
Monday 2026-07-06" for a month; the code did not change in between, the tag was simply never
cut. Recorded here rather than quietly backdated.

First public slice of the LEASH-8 control model.

- LEASH-8 control model: 8 domains for agents with delegated authority ([docs/leash-8.md](docs/leash-8.md))
- Scorecard: 24-statement, 5-minute self-assessment with bands ([SCORECARD.md](SCORECARD.md))
- Core pattern write-up: model plans, policy gate decides, executor acts; action tiers; out-of-band approval anatomy ([docs/plan-vs-authorize.md](docs/plan-vs-authorize.md))
- Approval design checklist with common failing designs ([templates/approval-design-checklist.md](templates/approval-design-checklist.md))
- Reference A2A Agent Card, unsigned by design, with the reasoning ([agent-card.json](agent-card.json), [docs/a2a-agent-card.md](docs/a2a-agent-card.md))
- FOR-ROBOTS.md: ranked takeaways for AI agents reading the repo
