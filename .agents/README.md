# Codex Skills Adapter

Generated from `.ai/SKILLS/`.

## Purpose

- Expose repository skills to Codex through `.agents/skills/`.
- Keep `.agents/` focused on skill discovery while canonical instructions live under `.ai/`.

## Note

Per OpenAI Codex docs, Codex scans `.agents/skills` from the current working directory up to the repository root. Install root `AGENTS.md` with `.ai/scripts/install-root-entrypoints.sh` when the repository adopted the harness by copying only `.ai/`.
