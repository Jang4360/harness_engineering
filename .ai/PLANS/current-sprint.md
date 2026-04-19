# Current Sprint

## Goal

Adopt the template into a real repository while preserving the end-to-end loop.

## Structured state

- Narrative plan: this file
- Machine-readable progress: `.ai/PLANS/progress.json`
- Quality and readiness metrics: `.ai/EVALS/metrics.json`

## Think

- Clarify the product wedge and users.
- Identify what should stay generic versus what must become project-specific.

## Plan

- Decide project-specific commands for smoke, release, and rollback.
- Decide what extra ADRs or memory entries the real project needs immediately.

## Build

- Customize docs, runbooks, and commands.
- Adjust skills only where the default workflow is a poor fit.

## Review

- Confirm the chosen workflow still matches the team's actual delivery path.

## Test

- Run `scripts/verify.sh`.
- Run `scripts/smoke.sh` after configuring project-specific commands.

## Ship

- Confirm release and rollback runbooks are no longer template-level placeholders.

## Reflect

- Record what parts of the template were useful, noisy, or missing.
