---
name: career-ops
description: General Codex entrypoint for career-ops. Route plain-language job-search requests to the correct workflow and mode files.
---

# career-ops -- Router

Use this skill when the user asks what career-ops can do, pastes a JD or job URL without naming a workflow, or gives a plain-language request that needs routing.

## Route the Request

- pasted JD text or a job URL with no explicit mode -> `career-ops-auto-pipeline`
- scan new roles -> `career-ops-scan`
- process the inbox in `data/pipeline.md` -> `career-ops-pipeline`
- generate a tailored CV PDF -> `career-ops-pdf`
- inspect or update the tracker -> `career-ops-tracker`
- prepare or run batch processing -> `career-ops-batch`
- evaluate only -> load `modes/oferta.md`
- compare offers -> load `modes/ofertas.md`
- LinkedIn outreach -> load `modes/contacto.md`
- deep company research -> load `modes/deep.md`
- training/course evaluation -> load `modes/training.md`
- project evaluation -> load `modes/project.md`
- live application help -> load `modes/apply.md`

## Shared Loading Rules

1. Read `AGENTS.md`.
2. Read `config/profile.yml` if it exists.
3. Read `modes/_profile.md` if it exists.
4. Resolve `language.modes_dir` from `config/profile.yml` when present.
5. Use translated files from that directory when they exist; otherwise fall back to `modes/`.

## Mode Loading

- Shared plus mode file:
  `auto-pipeline`, `oferta`, `ofertas`, `contacto`, `apply`, `pipeline`, `scan`, `batch`, `pdf`
- Mode file only:
  `tracker`, `deep`, `training`, `project`

When the user simply wants the available workflows, summarize the skill names and the matching plain-language requests instead of showing slash commands.
