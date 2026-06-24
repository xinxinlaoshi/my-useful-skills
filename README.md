# my-useful-skills

Personal collection of reusable Codex/agent skills.

## Included skills

- `project-memory-steward/` — maintains project-local memory, state, runbooks, error logs, experiment notes, and data schemas for long-running research/engineering projects.

## Use a skill locally

Clone this repository:

```bash
git clone https://github.com/xinxinlaoshi/my-useful-skills.git
cd my-useful-skills
```

Install the skill at user level:

```bash
mkdir -p ~/.codex/skills/project-memory-steward
cp -r project-memory-steward/* ~/.codex/skills/project-memory-steward/
```

Or install inside a project repository:

```bash
mkdir -p .codex/skills/project-memory-steward
cp -r project-memory-steward/* .codex/skills/project-memory-steward/
```
