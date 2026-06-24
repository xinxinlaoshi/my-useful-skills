# project-memory-steward

A Codex/agent skill for maintaining project-local memory, state, runbooks, error logs, experiment notes, and data schemas.

Important boundary: this skill treats `AGENTS.md` and `CLAUDE.md` as read-only. It must not create or edit them. If agent guidance should change, it writes a proposal to `AGENTS_CHANGE_REQUEST.md` instead.

## Install

Project-local install:

```bash
mkdir -p .codex/skills/project-memory-steward
cp -r project-memory-steward/* .codex/skills/project-memory-steward/
```

User-level install:

```bash
mkdir -p ~/.codex/skills/project-memory-steward
cp -r project-memory-steward/* ~/.codex/skills/project-memory-steward/
```

## Example prompts

```text
Use project-memory-steward. Update PROJECT_STATE.md, RUNBOOK.md, and ERROR_LOG.md based on the latest run logs and manifests. Do not launch any large job or change AGENTS.md.
```

```text
Use project-memory-steward. This phase is done. Reconcile project state and write any AGENTS.md suggestions to AGENTS_CHANGE_REQUEST.md. Do not modify AGENTS.md.
```
