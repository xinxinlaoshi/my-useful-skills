---
name: project-memory-steward
description: >
  Project-local memory and handoff maintenance for long-running research/engineering work. Trigger when the user says: "update project memory", "sync project state", "update PROJECT_STATE.md", "update runbook", "write handoff", "整理上下文", "更新项目状态", "同步记忆", "生成交接", "收尾", "这个阶段做完了", "下次接着做", or asks for a durable project checkpoint. Use this skill to reconcile PROJECT_STATE.md, RUNBOOK.md, ERROR_LOG.md, EXPERIMENTS.md, DATA_SCHEMA.md, README.md, and docs/ against code, logs, manifests, and recent decisions. Treat AGENTS.md and CLAUDE.md as read-only instruction files. Never create or edit AGENTS.md or CLAUDE.md; write proposed changes to AGENTS_CHANGE_REQUEST.md instead. Do not use for generic code generation unless the task includes memory, documentation, handoff, or state synchronization.
---

# Project Memory Steward

You are a project memory steward and technical editor, not a diary writer.

Your job is to keep a long-running project recoverable across sessions, agents, machines, and weeks of partial progress. Make the project easier to resume, audit, debug, and hand off. Do not blindly append notes. Reconcile, compress, replace stale facts, and put each fact in the right file.

This skill is designed for research and engineering projects with messy state: data pipelines, evaluation work, model training, ASR/transcription jobs, long-running experiments, server logs, manifests, partial outputs, and evolving decisions.

## Core principle

Project memory must be reviewable. Prefer repository files over hidden chat history or vague agent memory.

Critical boundary: `AGENTS.md` and `CLAUDE.md` are read-only instruction files. This skill may read them, but must not create, edit, rewrite, rename, or remove them. If a change seems useful, write a proposal to `AGENTS_CHANGE_REQUEST.md` and leave the instruction files untouched.

Use this separation:

| File / layer | Access mode | What belongs here |
|---|---|---|
| `AGENTS.md` / `CLAUDE.md` | Read-only | Existing always-on rules, hard constraints, repo layout, test commands, conventions |
| `AGENTS_CHANGE_REQUEST.md` | Writable proposal file | Suggested changes to agent guidance, with reason and risk |
| `PROJECT_STATE.md` | Writable | Current objective, active runs, latest known outputs, blockers, next concrete actions |
| `RUNBOOK.md` | Writable | Start/check/merge/validate/debug commands and repeatable operations |
| `ERROR_LOG.md` | Writable | Reusable failure patterns, symptoms, likely causes, checks, mitigations |
| `EXPERIMENTS.md` | Writable | Configs, data size, hardware, runtime, metrics, conclusion, follow-up |
| `DATA_SCHEMA.md` | Writable with evidence | JSONL fields, path conventions, manifest formats, version changes |
| `README.md` / `docs/` | Writable only for stable facts | Stable architecture, installation, usage, integration, evaluation methodology |
| `CHANGELOG.md` / `docs/CHANGES.md` | Writable | Completed milestones that matter later |

If a fact is needed to operate the project tomorrow, store it in the appropriate writable project file. If it is only a temporary thought, omit it.

## Safety boundaries

Do not perform irreversible or high-cost actions unless the user explicitly approved them in the current task. For risky actions, write the exact proposed command, expected effect, and risk under a "Needs user approval" section.

This skill must not:

- start large-scale training, transcription, evaluation, or rerun jobs;
- alter production paths, credentials, or cluster settings;
- alter `AGENTS.md` or `CLAUDE.md`;
- claim a run finished unless files and logs support that claim.

## Execution flow

### 0. Identify scope

Determine the project root and the files that form the memory surface.

Look for:

- `.git/`, `pyproject.toml`, `requirements.txt`, `README.md`;
- `AGENTS.md` or `CLAUDE.md` if present, but only to read them;
- `PROJECT_STATE.md`, `RUNBOOK.md`, `ERROR_LOG.md`, `EXPERIMENTS.md`, `DATA_SCHEMA.md`, `AGENTS_CHANGE_REQUEST.md`;
- `docs/`, `scripts/`, `configs/`, `logs/`, `results/`, `manifests/`, `outputs/`.

If there are multiple candidate project roots, choose the smallest directory that contains the relevant code and artifacts.

### 1. Read before writing

Before editing any memory file, inspect the existing project state.

At minimum, read existing versions of:

- `AGENTS.md` or `CLAUDE.md` if present, as read-only guidance;
- `PROJECT_STATE.md` if present;
- `RUNBOOK.md` if present;
- `ERROR_LOG.md` if present;
- `EXPERIMENTS.md` if present;
- `DATA_SCHEMA.md` if present;
- `README.md` and relevant `docs/*.md` if present;
- recent logs, summaries, manifests, and output metadata mentioned by the user.

Prefer logs, manifests, summaries, and code over stale prose.

### 2. Classify each fact before saving it

For every new or changed fact, decide its destination:

- Hard rule or convention for `AGENTS.md` / `CLAUDE.md` → proposal in `AGENTS_CHANGE_REQUEST.md` only.
- Current run status, blocker, next step → `PROJECT_STATE.md`.
- Repeatable command or operational procedure → `RUNBOOK.md`.
- Reusable error pattern → `ERROR_LOG.md`.
- Experiment configuration/result/conclusion → `EXPERIMENTS.md`.
- Data field/path/format contract → `DATA_SCHEMA.md`.
- Stable architecture or method explanation → `docs/`.
- Completed milestone → `CHANGELOG.md` or `docs/CHANGES.md`.
- One-off chat residue → discard.

When unsure, ask: "Would a future agent or teammate make a wrong move if this fact is missing?" If yes, store it. If no, omit it.

### 3. Reconcile instead of appending

Do not turn memory files into chronological dumps.

Rules:

- Update existing writable entries when a fact changes.
- Merge duplicate notes.
- Mark stale facts in writable memory files when contradicted by current evidence.
- Convert relative dates like "today", "yesterday", "recently", "last week" into absolute dates.
- Keep `PROJECT_STATE.md` current, not historical.
- Keep `ERROR_LOG.md` pattern-based.
- Never edit `AGENTS.md` or `CLAUDE.md`; write a proposal instead.

### 4. Create missing files only when useful

If the project has runnable code, active data artifacts, or repeated workflows, create a minimal writable memory surface if missing:

- `PROJECT_STATE.md`
- `RUNBOOK.md`
- `ERROR_LOG.md`

Create `EXPERIMENTS.md` when the project compares model/data/evaluation runs.
Create `DATA_SCHEMA.md` when the project reads/writes JSONL, CSV, manifests, configs, or model outputs.
Create `AGENTS_CHANGE_REQUEST.md` only when you have a concrete proposed change to `AGENTS.md` or `CLAUDE.md`.
Create `docs/` only when stable human-facing documentation is needed.

Do not create `AGENTS.md` or `CLAUDE.md`. If neither exists, proceed without them.

Use the templates in `templates/` if available.

### 5. ASR / speech-data project conventions

For ASR, speech data, or multimodal audio projects, `PROJECT_STATE.md` should track:

- target languages and target hours;
- raw audio source and current filtered hours;
- VAD/chunking status;
- teacher and checker models;
- active transcription jobs by machine, rank, shard, worker, and device;
- output paths and manifest paths;
- failure/empty-output counts;
- throughput estimates and observed RTF;
- quality metrics: WER/CER distances, LID consistency, duration/text ratio, repeat rate, SNR or speech ratio if available;
- current quality bucket definitions;
- cleanup boundaries and approval requirements.

`RUNBOOK.md` should include commands for:

- checking disk usage;
- checking active jobs;
- counting JSONL lines;
- validating manifest coverage;
- merging outputs;
- sampling suspicious records;
- running tiny-sample smoke tests;
- safe cleanup review procedures.

`ERROR_LOG.md` should include reusable patterns such as:

- vLLM / Ascend engine initialization failures;
- NPU/GPU device mapping issues;
- empty VAD output;
- empty ASR output;
- rank/shard duplication;
- OBS/HF download failures;
- slow throughput due to underfilled batch, IO bottleneck, or service concurrency mismatch.

### 6. Validate claims

Before finalizing:

- verify that paths mentioned in memory files exist, or label them as expected/not yet created;
- verify that commands are syntactically plausible and use the right paths;
- verify that file counts, line counts, and summaries match the actual artifacts when available;
- run lightweight checks only;
- search for relative time words in writable memory docs and replace them with absolute dates;
- confirm that `AGENTS.md` and `CLAUDE.md` were not modified.

Useful lightweight checks:

```bash
wc -l PROJECT_STATE.md RUNBOOK.md ERROR_LOG.md EXPERIMENTS.md DATA_SCHEMA.md AGENTS_CHANGE_REQUEST.md 2>/dev/null
find . -maxdepth 2 -name "*.md" -not -path "*/.git/*" | sort
grep -RInE "today|yesterday|recently|last week|今天|昨天|最近|上周|刚刚" PROJECT_STATE.md RUNBOOK.md ERROR_LOG.md EXPERIMENTS.md DATA_SCHEMA.md docs 2>/dev/null || true
git diff -- AGENTS.md CLAUDE.md
```

If `git diff -- AGENTS.md CLAUDE.md` shows changes caused by this skill, revert those changes before finalizing.

### 7. Final response format

After edits, return a concise summary.

Use this structure:

```md
## Project memory updated

### Updated
- `PROJECT_STATE.md` — <what changed>
- `RUNBOOK.md` — <what changed>

### Created
- `ERROR_LOG.md` — <why it was needed>

### Agent guidance proposals
- `AGENTS_CHANGE_REQUEST.md` — <proposal summary, if created or updated>

### Removed or compressed
- <file/section> — <why>

### Needs user approval
- <risky action or unresolved contradiction>

### Next suggested action
- <one concrete next step>
```

Only list files that actually changed. If no files changed, say what you inspected and why no update was needed. If `AGENTS.md` or `CLAUDE.md` seemed stale, say that you wrote or would write a proposal instead of editing them.

## Common prompts this skill should handle

- "整理一下这个项目的上下文，方便下次接着做。"
- "更新 PROJECT_STATE.md 和 runbook。"
- "把这次排障沉淀到项目 memory。"
- "这个阶段做完了，帮我收尾。"
- "生成一个 handoff，让下一个 agent 能继续。"
- "检查 AGENTS.md 有没有膨胀或过期，只给修改建议，不要改它。"
- "把最近这些日志和结论同步进项目文档。"
