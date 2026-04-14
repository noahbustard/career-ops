---
name: career-ops-pipeline
description: Process pending job URLs from data/pipeline.md, run the full evaluation flow, and move completed items into the processed section.
---

# career-ops-pipeline

Use this skill when the user wants to process the inbox in `data/pipeline.md`.

## Load

1. `AGENTS.md`
2. `config/profile.yml` if present
3. `modes/_profile.md` if present
4. resolved `_shared.md`
5. resolved `pipeline.md`

## Execution Rules

- If setup files are missing, onboard first.
- Use the auto-pipeline flow for each pending item.
- If there are many pending URLs, delegation can help but is optional.
- Preserve the file format of `data/pipeline.md`.
- Keep tracker writes consistent with repo rules.
