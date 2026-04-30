# Architecture

## Canonical layers

- `.ai/` is the methodology and project-operations layer.
- `.ai/SKILLS/` is the canonical workflow layer.
- `.ai/ADAPTERS/` is the canonical runtime-adapter template layer.
- `.ai/scripts/` is the canonical deterministic-automation layer.
- `.ai/README.md`, `.ai/AGENTS.md`, and `.ai/CLAUDE.md` are the canonical host-facing documentation layer.
- `.ai/PLANS/progress.json` is the canonical structured progress layer.
- `.ai/EVALS/metrics.json` plus promotion and retry logs are the canonical measurable state layer.
- `.ai/GUARDS.md`, `.ai/PROMOTION.md`, and `.ai/AUTOMATION.md` define harness control behavior.
- `.claude/skills/` is the Claude adapter layer generated from canonical skills.
- `.agents/skills/` is the Codex adapter layer generated from canonical skills.
- `.codex/` holds generated repo-local Codex configuration placeholders.

## Design intent

This template now separates the portable harness core from repo-root entrypoints.

- The portable unit is `.ai/`.
- A repository can adopt the harness by copying only `.ai/`.
- Root entrypoints such as `AGENTS.md` and `CLAUDE.md` are install surfaces, not the design source.
- Generated adapters remain rebuildable views over canonical `.ai/` sources.

## Harness control model

The harness is a control system for AI-assisted delivery, not a prompt collection. It is built from three cooperating parts:

- **Agents**: role-scoped workers with explicit responsibilities, tool access, and permission boundaries.
- **Skills**: reusable operating manuals that describe when to act, what inputs to load, what procedure to follow, and what durable output to leave behind.
- **Orchestrator**: the stage loop, `.ai/scripts/`, and the human or agent operator that route work through planning, implementation, review, QA, release, and learning.

The operating philosophy is: when an agent repeats a mistake, strengthen the system boundary instead of only rewriting the prompt. Examples include blocking dangerous commands in code, denying direct access to sensitive files or systems, forcing plan readiness checks before build, and routing Codex implementation through a separate review handoff.

## Data flow

1. A request or event is classified against the current sprint and workflow.
2. The orchestrator chooses the relevant stage and loads the matching skill.
3. Humans or agents update canonical docs and canonical skills under `.ai/`.
4. Planning and implementation update narrative plan artifacts plus `.ai/PLANS/progress.json`.
5. Guards and validation scripts under `.ai/scripts/` check command risk, TDD discipline, plan readiness, code syntax, and adapter sync before work is treated as ready.
6. Evaluation and release update `.ai/EVALS/metrics.json` and related logs.
7. Promotion decisions update `.ai/MEMORY/`, `.ai/SKILLS/`, `.ai/EVALS/`, or `.ai/DECISIONS/`.
8. `.ai/scripts/sync-adapters.sh` copies canonical skills and runtime adapter templates into `.claude/`, `.agents/`, and `.codex/`.
9. `.ai/scripts/install-root-entrypoints.sh` can install root `AGENTS.md`, `CLAUDE.md`, and an optional `README.md` pointer for hosts that require root discovery.
10. Claude uses `.claude/skills/`, root `CLAUDE.md`, and generated `.claude/settings.json`.
11. Codex uses `.agents/skills/`, root `AGENTS.md`, and generated `.codex/` placeholders.
12. `.ai/scripts/dashboard.sh` turns canonical progress, metrics, and promotion state into a visible status summary.

## Trust boundaries

- Canonical policy belongs in `.ai/`.
- Root entrypoint files are installable host-discovery shims over canonical docs.
- Generated adapters are rebuildable runtime views, not design sources.
- Local-only host files such as `.claude/settings.local.json` may exist for a developer, but they must stay ignored and untracked.
- Verification should fail on tracked policy drift, invalid structure, unsynced adapters, or code validation failures. It should not fail merely because an ignored local host file exists.

## Change policy

- Change canonical skill behavior only under `.ai/SKILLS/`.
- Change canonical host instructions in `.ai/AGENTS.md` and `.ai/CLAUDE.md`.
- Change harness command behavior only under `.ai/scripts/`.
- Regenerate adapters after canonical changes.
