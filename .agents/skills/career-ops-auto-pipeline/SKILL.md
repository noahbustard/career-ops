---
name: career-ops-auto-pipeline
description: Full job workflow for a pasted JD or job URL: extract the JD, evaluate the role, generate a report and PDF, and prepare tracker output.
---

# career-ops-auto-pipeline

Use this skill when the user pastes a job description or job URL and wants the full workflow.

## Load

1. `AGENTS.md`
2. `config/profile.yml` if present
3. `modes/_profile.md` if present
4. resolved `_shared.md`
5. resolved `auto-pipeline.md`

If the mode directs you to other files such as `oferta.md` or `pdf.md`, load them too and follow their instructions.

## Execution Rules

- If required setup files are missing, onboard first.
- Use Playwright for JD extraction and active-offer verification whenever the runtime supports it.
- Preserve the report, PDF, and tracker conventions from the repo.
- Strongly discourage low-fit roles below `4.0/5`.
- Never submit an application without user review.
