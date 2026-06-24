# RUNBOOK.md

## Start / resume
```bash
# Fill in project-specific resume command.
<command>
```

## Check active jobs
```bash
# Fill in project-specific job check command.
<command>
```

## Validate outputs
```bash
# Fill in project-specific output validation command.
<command>
```

## Safe cleanup
Never run destructive cleanup directly. Prefer quarantine moves first.

```bash
# Example pattern: move candidate directory aside before deletion.
mv <path> <path>.to_delete.YYYYMMDD_HHMMSS
```
