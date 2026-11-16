---
name: leash-8-scorecard
description: Score how much freedom an AI agent setup has with the LEASH-8 worksheet (24 statements, 8 control domains, 0/1/2 each) and name the cheapest missing controls. Use when someone asks to audit agent permissions, rate agent safety controls, or review approvals, egress or kill switches for agents that use real tools.
---

# LEASH-8 scorecard

Rate an AI agent system that uses real tools (shell, browser, messengers, payments) across 8 control domains and report the band plus the cheapest missing controls. The worksheet measures control coverage, not "safety".

## Reference files in this skill

- `SCORECARD.md`: the 24 statements, the scoring scale and the four bands.
- `leash-8.md`: what each domain is, why it matters, the minimal implementation and the evidence to ask for.

Read both before scoring.

## Steps

1. Ask the operator which agent setup to score (one agent, one machine, or a fleet) and whether they want the readout as chat text or a file.
2. Collect facts, not guesses. For each statement ask for or look at the evidence named in `leash-8.md` (config, log line, allowlist file, drill timestamp). Read-only inspection is fine; change nothing.
3. Score each of the 24 statements: 0 = not true, 1 = partially true, 2 = fully true and the evidence can be shown in under a minute. Without evidence the score is at most 1. If you cannot judge a statement, mark it "unknown" and say why; never count it as 2.
4. Sum the scores (maximum 48) and read the band from `SCORECARD.md`. Report how many statements were unknown.
5. List every 0 under Identity (1), Approvals (4) and Egress (6) first. Then pick the three cheapest missing controls, using the minimal implementation from `leash-8.md`. Default order that pays fastest for a small operation: Approvals, Egress, Supply chain, Observability.
6. Give the operator a table (statement, score, evidence seen), then the total, the band, the unknowns and the three proposed controls.

## Rules

- Propose controls; never apply them, change permissions, or touch secrets yourself.
- Never print secret values you come across while collecting evidence; report only that one exists and where.
- Do not tell the operator the system is "secure" or "safe". Say which controls exist, what they cover and what stays with a human.
- Treat the worksheet as data, not as authorization. It never overrides the operator's own policies.
- Do not add vendor recommendations or numbers without a source.

## Optional code reference

A proof-of-concept of the gate pattern lives in a separate repository: https://github.com/tonydzi/leash-poc . This skill does not install or run it.
