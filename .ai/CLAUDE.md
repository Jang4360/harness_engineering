# CLAUDE.md

Use this repository as a structured AI harness, not as a loose prompt sandbox.

## Canonical sources

- Read `.ai/PROJECT.md`, `.ai/ARCHITECTURE.md`, and `.ai/WORKFLOW.md` before major changes.
- Treat `.ai/` as canonical.
- Treat `.claude/skills/` as generated adapter output.
- Treat `.claude/settings.json` as generated from `.ai/ADAPTERS/claude/settings.json`.
- Use `.ai/PLANS/progress.json` and `.ai/EVALS/metrics.json` when summarizing status.
- If the repository only copied `.ai/`, run `.ai/scripts/install-root-entrypoints.sh` so Claude can discover root `CLAUDE.md`.

## Skills

- Claude-compatible skills live under `.claude/skills/`.
- Update `.ai/SKILLS/` first, then run `.ai/scripts/sync-adapters.sh`.
- Prefer the stage-appropriate skill instead of improvising a new workflow mid-task.
- Use the `dashboard` skill or `.ai/scripts/dashboard.sh` for visible status.

## Workflow discipline

- Move work through Think -> Plan -> Build -> Review -> Test -> Ship -> Reflect.
- Write durable artifacts back to `.ai/` when the work changes scope, architecture, risk, release readiness, or learnings.
- If a result should survive the session, store it in `.ai/MEMORY/`, `.ai/EVALS/`, `.ai/PLANS/`, or `.ai/DECISIONS/`.
- Promotion decisions should follow `.ai/PROMOTION.md`.

## Expected checks

- Run `.ai/scripts/verify.sh` after structural changes.
- Run `.ai/scripts/sync-adapters.sh` after canonical skill changes.
- Run `.ai/scripts/update-progress.sh` after changing item statuses in `progress.json`.
- Run `.ai/scripts/update-metrics.sh` after retry, promotion, or blocker state changes.
- Use `.ai/scripts/smoke.sh` after project-specific commands are configured.
- Use the guard scripts before enabling runtime hooks for command execution or repeated retries.
