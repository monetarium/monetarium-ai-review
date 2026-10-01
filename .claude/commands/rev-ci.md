---
description: Ponytail-review in an isolated agent, then apply the findings here
argument-hint: [what to review — path, PR, diff; empty = uncommitted changes]
---

Run a review of the target below in an **isolated subagent** using the `ponytail-review` skill, then apply what it returns in this session via `receiving-code-review`.

Target: $ARGUMENTS
(empty → the uncommitted changes in the repo the target lives in)

- Subagent model: Opus for judgement-heavy reviews; otherwise Sonnet. Never above the session's model: under a Sonnet session, Sonnet. State the choice before launching.
- Tell the subagent to return findings as a list: `file:line — what to cut — what replaces it`. No patches.
- On return, apply `receiving-code-review`: verify each finding against the code before implementing it.
