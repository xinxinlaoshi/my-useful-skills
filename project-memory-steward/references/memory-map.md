# Memory destination map

Use this map when deciding where a fact belongs.

Important boundary: `AGENTS.md` and `CLAUDE.md` are read-only. This skill may read them, but must not change them.

| Fact type | Destination | Example |
|---|---|---|
| Existing always-on agent rule | Read `AGENTS.md` / `CLAUDE.md`; do not edit | "Require explicit approval before risky operations." |
| Suggested agent rule change | `AGENTS_CHANGE_REQUEST.md` | "Proposal: add a rule requiring tiny-sample validation before full reruns." |
| Current active status | `PROJECT_STATE.md` | "rank0_of4 is complete; rank1_of4 has empty outputs." |
| Reusable command | `RUNBOOK.md` | "Count output lines with `wc -l results/*.jsonl`." |
| Reusable failure pattern | `ERROR_LOG.md` | "vLLM-Ascend engine init fails during multi-worker startup." |
| Experiment result | `EXPERIMENTS.md` | "6xL40 processed 1000h in 10h; likely underfilled service." |
| Data contract | `DATA_SCHEMA.md` | "`duration_sec` is required; `lid_confidence` is optional." |
| Stable system explanation | `docs/architecture.md` | "VAD -> ASR -> consistency scoring -> filtering." |
| Historical milestone | `CHANGELOG.md` | "2026-06-22: First Russian filtering pass completed." |
| Temporary chat thought | Omit | "Maybe try another idea later." |

Default rule: if missing information would cause a wrong command, wrong rerun, wrong threshold, or wrong report, store it in a writable project-memory file. If it would require changing agent guidance, write a proposal instead of changing the instruction file.
