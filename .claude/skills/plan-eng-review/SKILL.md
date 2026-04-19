---
name: plan-eng-review
description: Force architecture, data flow, failure modes, trust boundaries, and test strategy into the open before implementation.
---

# plan-eng-review

## purpose

Turn product intent into an execution plan that engineers and QA can actually follow.

## when to use

- After the product wedge and scope are understood
- Before large implementation work
- When a change touches architecture, external systems, state, or security boundaries

## inputs

- `.ai/PLANS/current-sprint.md`
- `.ai/ARCHITECTURE.md`
- `.ai/EVALS/failure-patterns.md`
- Relevant ADRs or incidents if they exist

## procedure

1. Map the proposed flow: trigger, data movement, state changes, and external boundaries.
2. Identify failure modes, race conditions, stale state risks, and trust boundaries.
3. Define the test strategy for happy path, edge cases, and regressions.
4. Document implementation constraints and open questions in `.ai/PLANS/current-sprint.md`.
5. Update `.ai/ARCHITECTURE.md` or draft an ADR when the system shape changes.

## outputs

- Engineering review notes
- Data flow and failure mode summary
- Trust boundary notes
- Test strategy

## escalation rules

- Escalate if the architecture depends on assumptions that are not yet validated.
- Escalate if production risk is high and no rollback or monitoring path exists.

## handoff rules

- Hand off to `plan-design-review` if UX states still need clarification.
- Hand off to build skills once architecture and test strategy are explicit enough to execute.
