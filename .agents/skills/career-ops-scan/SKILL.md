---
name: career-ops-scan
description: Scan configured portals, verify live offers, dedupe against history and tracker data, and append new matches to data/pipeline.md.
---

# career-ops-scan

Use this skill when the user wants to discover new offers from `portals.yml`.

## Load

1. `AGENTS.md`
2. `config/profile.yml` if present
3. `modes/_profile.md` if present
4. resolved `_shared.md`
5. resolved `scan.md`
6. `portals.yml`
7. `data/scan-history.tsv` if present
8. `data/pipeline.md` if present
9. `data/applications.md` if present

## Execution Rules

- If `portals.yml` is missing, onboard first.
- Prefer direct careers pages plus Playwright.
- Deduplicate against scan history, tracker, and pipeline.
- Verify web-search-only discoveries for liveness before adding them.
- Write new roles into `data/pipeline.md` and record outcomes in `data/scan-history.tsv`.
