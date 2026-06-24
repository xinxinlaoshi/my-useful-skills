# AGENTS.md

## Project
<one-paragraph description of the project>

## Hard rules
- Do not delete data, logs, checkpoints, manifests, or output directories unless explicitly approved.
- Do not launch large-scale training, transcription, evaluation, or rerun jobs without showing the exact command and expected output path.
- Do not change production paths, credentials, or cluster settings without approval.
- For every Python or shell script, start with one comment line explaining the script purpose.
- Prefer tiny-sample validation before full-scale execution.

## Repo layout
- `<path>` — <purpose>

## Verification
Before claiming completion:
1. Run syntax checks if code changed.
2. Run a tiny sample when feasible.
3. Report exact command, input size, output path, and observed result.
4. Update `PROJECT_STATE.md` and relevant memory files when project state changes.
