# Changelog

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
