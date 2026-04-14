---
name: career-ops-pdf
description: Generate an ATS-optimized, job-tailored CV PDF from cv.md, profile data, and a target JD.
---

# career-ops-pdf

Use this skill when the user wants only the tailored PDF workflow.

## Load

1. `AGENTS.md`
2. `config/profile.yml` if present
3. `modes/_profile.md` if present
4. resolved `_shared.md`
5. resolved `pdf.md`
6. `templates/cv-template.html`

## Execution Rules

- If `cv.md` or `config/profile.yml` is missing, onboard first.
- Read the JD before generating the PDF.
- Never invent skills or metrics.
- Write PDFs to `output/` using the repo naming conventions.
