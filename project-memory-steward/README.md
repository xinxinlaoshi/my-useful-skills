# project-memory-steward

A Codex/agent skill for maintaining project-local memory, state, runbooks, error logs, experiment notes, and data schemas.

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
Use project-memory-steward. Update PROJECT_STATE.md, RUNBOOK.md, and ERROR_LOG.md based on the latest run logs and manifests. Do not launch any large job or delete data.
```

```text
Use project-memory-steward. This phase is done. Reconcile AGENTS.md and project docs so the next agent can resume safely.
```
