# Setup Guide

README.md is the shortest path. This page adds the exact file setup and Codex usage details.

## Prerequisites

- Codex CLI installed and signed in
- Node.js 18+
- `npx playwright install chromium`
- Go 1.21+ only if you want the optional dashboard TUI

## User-Layer Setup

Copy the user templates:

```bash
cp config/profile.example.yml config/profile.yml
cp modes/_profile.template.md modes/_profile.md
cp templates/portals.example.yml portals.yml
```

Then add:
- `cv.md`
- optional `article-digest.md`

Recommended order:
1. fill `config/profile.yml`
2. add `cv.md`
3. update `modes/_profile.md` with your own positioning
4. customize `portals.yml`

## Validate

```bash
npm run doctor
node cv-sync-check.mjs
node verify-pipeline.mjs
```

## Start Codex

Interactive:

```bash
cd /path/to/career-ops
codex
```

One-shot:

```bash
codex -C /path/to/career-ops "Use the career-ops-auto-pipeline skill and evaluate this job URL: https://example.com/jobs/123"
```

## Core Workflows

| Workflow | Codex prompt |
|----------|--------------|
| Full evaluation | `Use the career-ops-auto-pipeline skill. Evaluate this JD end to end: ...` |
| Scan portals | `Use the career-ops-scan skill. Scan portals.yml and update data/pipeline.md.` |
| Process pending URLs | `Use the career-ops-pipeline skill. Process data/pipeline.md.` |
| Generate PDF | `Use the career-ops-pdf skill. Generate an ATS-optimized PDF for this JD: ...` |
| Tracker | `Use the career-ops-tracker skill. Show tracker stats.` |
| Batch | `Use the career-ops-batch skill. Run the batch workflow.` |

## Recurring Runs

For repeated scans, do one of these:
- create a Codex app automation that opens this repo and uses `career-ops-scan`
- schedule `codex exec` from cron or another task runner

Example:

```bash
cd /path/to/career-ops && codex exec --dangerously-bypass-approvals-and-sandbox --search "Use the career-ops-scan skill. Read AGENTS.md, scan portals.yml, update data/scan-history.tsv and data/pipeline.md, then summarize the changes."
```

## Dashboard

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard
```
