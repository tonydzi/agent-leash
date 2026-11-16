# agent-leash: LEASH-8 scorecard plugin

This instructions-only plugin adds one skill, `leash-8-scorecard`. It helps Claude score an AI agent setup against the LEASH-8 control model: 24 statements across 8 domains (Identity, Secrets, Tool and skill supply chain, Approvals, Containment, Egress, Observability, Incident response), each rated 0, 1 or 2.

The result is a total out of 48, a band (Free-running, Loose leash, On a leash, Operator-grade), the list of zero scores in Identity, Approvals and Egress, and the three cheapest missing controls. The worksheet measures control coverage, not safety.

## Install and use

```text
/plugin marketplace add tonydzi/agent-leash
/plugin install agent-leash@agent-leash
```

Then ask Claude, for example: "Score my agent setup with LEASH-8" or "Which safety controls is my agent missing?". The skill asks which setup to score, collects evidence, scores each statement and proposes (never applies) the next controls.

## What is inside

- `skills/leash-8-scorecard/SKILL.md`: the scoring procedure.
- `skills/leash-8-scorecard/SCORECARD.md`: the worksheet and bands.
- `skills/leash-8-scorecard/leash-8.md`: the control model with minimal implementations and evidence per domain.

The plugin contains only Markdown, JSON and an icon. It runs no code, starts no server and declares no remote endpoint. A proof-of-concept of the gate pattern is kept separately at [tonydzi/leash-poc](https://github.com/tonydzi/leash-poc). Privacy: [PRIVACY.md](https://github.com/tonydzi/agent-leash/blob/main/PRIVACY.md).

License: MIT.
