# Architecture

## System Overview

```text
                    ┌────────────────────────────────┐
                    │          Codex Session         │
                    │  (reads AGENTS + repo skills + │
                    │          modes/*.md)           │
                    └──────────────┬─────────────────┘
                                   │
             ┌─────────────────────┼─────────────────────┐
             │                     │                     │
      ┌──────▼──────┐      ┌──────▼──────┐      ┌──────▼────────┐
      │ Auto-Pipeline│      │ Portal Scan │      │ Batch Runner  │
      │ / Single Eval│      │   scan.md   │      │ batch-runner  │
      └──────┬──────┘      └──────┬──────┘      └──────┬────────┘
             │                     │                     │
             │              ┌──────▼──────┐       ┌─────▼──────┐
             │              │ pipeline.md │       │ codex exec │
             │              │  URL inbox  │       │  workers   │
             │              └─────────────┘       └─────┬──────┘
             │                                           │
      ┌──────▼───────────────────────────────────────────▼──────┐
      │                    Output Pipeline                       │
      │  Report (.md)   PDF (.pdf)   Tracker TSV   Merge/verify │
      └──────────────────────────────────────────────────────────┘
                                   │
                         ┌─────────▼──────────┐
                         │ data/applications.md│
                         │ canonical tracker   │
                         └─────────────────────┘
```

## Single-Offer Flow

1. User pastes JD text or a job URL
2. Codex loads the relevant mode files
3. JD is extracted with Playwright or fallback fetch/search
4. Career-Ops runs the A-F evaluation blocks
5. A report is written to `reports/`
6. A tailored PDF is generated in `output/`
7. A tracker TSV line is written to `batch/tracker-additions/`
8. `merge-tracker.mjs` merges that line into `data/applications.md`

## Batch Processing

```text
batch-input.tsv -> batch-runner.sh -> N x codex exec workers
                      │
                 batch-state.tsv
```

Each worker receives the resolved `batch/batch-prompt.md`, runs in the repo root, and produces:
- report markdown
- tailored PDF
- tracker TSV line
- JSON summary to stdout

The runner handles retries, resume, and post-run merge/verification.

## Data Flow

```text
cv.md                       -> candidate source of truth
article-digest.md           -> richer proof points
config/profile.yml          -> identity, targeting, comp, narrative
modes/_profile.md           -> user-specific framing
portals.yml                 -> scanner configuration
templates/states.yml        -> canonical status definitions
templates/cv-template.html  -> HTML source for PDF generation
```

## File Naming

- reports: `{###}-{company-slug}-{YYYY-MM-DD}.md`
- PDFs: `cv-candidate-{company-slug}-{YYYY-MM-DD}.pdf`
- tracker additions: `batch/tracker-additions/{id}.tsv`

## Integrity Scripts

| Script | Purpose |
|--------|---------|
| `merge-tracker.mjs` | merge batch TSV additions into the tracker |
| `verify-pipeline.mjs` | verify statuses, duplicates, and links |
| `dedup-tracker.mjs` | remove duplicate tracker entries |
| `normalize-statuses.mjs` | normalize aliases to canonical status values |
| `cv-sync-check.mjs` | validate CV/profile setup consistency |

## Dashboard

The Go TUI under `dashboard/` visualizes the current pipeline with filters, sorting, previews, and inline status changes.
