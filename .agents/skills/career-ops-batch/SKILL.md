---
name: career-ops-batch
description: Review, prepare, or run the parallel batch workflow using batch/batch-input.tsv and the codex exec-based batch runner.
---

# career-ops-batch

Use this skill when the user wants to process many offers in parallel.

## Load

1. `AGENTS.md`
2. `config/profile.yml` if present
3. `modes/_profile.md` if present
4. resolved `_shared.md`
5. resolved `batch.md`
6. `batch/batch-prompt.md`
7. `batch/batch-runner.sh`
8. `batch/batch-input.tsv` if present
9. `batch/batch-state.tsv` if present

## Execution Rules

- If the user wants execution, run `batch/batch-runner.sh` rather than inventing a new workflow.
- Keep report numbering, tracker TSV output, and post-run merge behavior consistent with the repo.
- Use the runner’s dry-run or retry modes when they are the safer fit.
