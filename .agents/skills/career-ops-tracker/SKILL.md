---
name: career-ops-tracker
description: Inspect tracker status, summarize application metrics, and update existing tracker rows when the user asks.
---

# career-ops-tracker

Use this skill when the user wants to inspect or update the application tracker.

## Load

1. `AGENTS.md`
2. resolved `tracker.md`
3. `data/applications.md` if present

## Execution Rules

- Never add new rows directly to `data/applications.md`.
- Updating status or notes on existing rows is allowed.
- Respect canonical statuses from `templates/states.yml`.
- Summaries should include totals, status counts, average score, and PDF/report coverage when possible.
