# Prototype (v3): The Build That Tests the Hypothesis

> Module 2 · Validation. The prototype is a hypothesis test, not a demo.

## Link

https://acme-retention.lovable.app

## What it tests

_Tie it back to the validation brief: which assumption does this prototype put in front of a user?_

The Activation prototype tests whether a guided first-week path helps a new account reach a team-level “aha” moment instead of signing up, exploring aimlessly, and leaving.

## Context injected (no placeholders)

- **Real user quotes on screen:** “I signed up, poked around for ten minutes, and never figured out what it actually did for my team.” / “Nobody on my team adopted it, so I stopped logging in. It felt like one more tool to babysit.”
- **Domain metrics on screen:** 30% 90-day churn: share of new accounts gone within 3 months. / 22% Activation rate: reach the "aha" action in week one. / 1.4 Seats active / account: adoption rarely spreads past the buyer.

## Iteration log (v1 → v3)

| Version | Change | Why |
|---|---|---|
| v1 | Make the plan-sharing step show a clear success state: name the teammate and the first action they now own. | That would make the “aha” feel like a real team outcome, not just a completed form. |
| v2 | Make the activation success state depend on the teammate claiming or completing their first assigned action, not just the plan being shared. | Right now, a new account can count as activated after entering a teammate’s name and sharing a simulated plan. Requiring the teammate to engage would test whether the flow solves the “nobody on my team adopted it” problem, and make the activation readout a stronger signal. |
| v3 | Remove the auto claim of the task | Right now, Casey claims it automatically after a short wait, so the flow moves to “Activated” without the tester deciding whether the teammate actually engages. A no-response choice would let you see whether the product handles the adoption problem that the Eng manager described—and whether users still find value when the handoff stalls. |
