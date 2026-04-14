# Career-Ops

> AI-powered job-search pipeline for Codex. Evaluate offers, generate tailored CV PDFs, scan portals, process batches, and keep the whole search in one repo.

![Codex](https://img.shields.io/badge/Codex-0A0A0A?style=flat&logo=openai&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

<p align="center">
  <img src="docs/demo.gif" alt="Career-Ops Demo" width="800">
</p>

## What It Does

Career-Ops turns Codex into a full job-search command center:
- evaluate offers with structured A-F scoring
- generate ATS-optimized CV PDFs tailored to each JD
- scan job portals and append new leads to the pipeline
- process many offers in batch with `codex exec` workers
- track reports, PDFs, statuses, and notes in one place

This is a filter, not a spray-and-pray system. It is designed to help you find the few roles worth serious attention, not to mass-apply everywhere. Always review before submitting.

Built by someone who used it to evaluate 740+ job offers, generate 100+ tailored CVs, and land a Head of Applied AI role. [Read the case study](https://santifer.io/career-ops-system).

## Prerequisites

- [Codex CLI](https://developers.openai.com/codex) installed and signed in
- Node.js 18+
- Playwright Chromium for PDF generation and browser-based parsing
- Go 1.21+ if you want the optional dashboard TUI

## Install

```bash
git clone https://github.com/santifer/career-ops.git
cd career-ops
npm install
npx playwright install chromium
```

## Set Up Your Files

Copy the user-layer templates, then add your own data:

```bash
cp config/profile.example.yml config/profile.yml
cp modes/_profile.template.md modes/_profile.md
cp templates/portals.example.yml portals.yml
```

Create these files:
- `cv.md` — your canonical CV in markdown
- `article-digest.md` — optional proof points, case studies, articles

Run the setup checker:

```bash
npm run doctor
```

## Start Codex In This Repo

Interactive:

```bash
cd /path/to/career-ops
codex
```

One-shot:

```bash
codex -C /path/to/career-ops "Use the career-ops-auto-pipeline skill and evaluate this URL: https://example.com/jobs/123"
```

The repo is Codex-first. The main entry points are the repo skills under `.agents/skills/` plus the instructions in `AGENTS.md`.

## Main Codex Skills

| Skill | Use it for |
|------|-------------|
| `career-ops` | General routing and discovery |
| `career-ops-auto-pipeline` | Full evaluation from pasted JD text or URL |
| `career-ops-scan` | Portal scanning into `data/pipeline.md` |
| `career-ops-pipeline` | Processing pending URLs from the pipeline inbox |
| `career-ops-pdf` | Tailored ATS CV PDF generation |
| `career-ops-tracker` | Viewing or updating application status |
| `career-ops-batch` | Reviewing or running parallel batch evaluation |

## Exact Usage Examples

Interactive prompts to paste into Codex:

```text
Use the career-ops-auto-pipeline skill. Evaluate this job end to end and produce the report, PDF, and tracker output: https://example.com/jobs/123
```

```text
Use the career-ops-scan skill. Read AGENTS.md, scan portals.yml, dedupe against scan history, applications, and pipeline, verify web-search hits are still live, then append new matches to data/pipeline.md.
```

```text
Use the career-ops-pipeline skill. Process every pending URL in data/pipeline.md and summarize the results in a table.
```

```text
Use the career-ops-pdf skill. Generate an ATS-optimized PDF for this JD and tell me which keywords were injected.
```

```text
Use the career-ops-tracker skill. Show tracker stats, then list the top roles scoring 4.0/5 or higher.
```

```text
Use the career-ops-batch skill. Review batch/batch-input.tsv, make sure the batch is ready, then run the batch runner with --parallel 2.
```

Equivalent one-shot CLI calls:

```bash
codex -C /path/to/career-ops "Use the career-ops-auto-pipeline skill. Evaluate this JD: https://example.com/jobs/123"
codex -C /path/to/career-ops "Use the career-ops-scan skill. Scan portals.yml and update data/pipeline.md."
codex -C /path/to/career-ops "Use the career-ops-pipeline skill. Process pending URLs in data/pipeline.md."
codex -C /path/to/career-ops "Use the career-ops-pdf skill. Generate a PDF for this JD: https://example.com/jobs/123"
codex -C /path/to/career-ops "Use the career-ops-tracker skill. Show tracker status and stats."
codex -C /path/to/career-ops "Use the career-ops-batch skill. Run the batch workflow."
```

## Batch Workflow

Batch runs are now Codex-native through `codex exec`.

Prepare `batch/batch-input.tsv`, then run:

```bash
npm run batch -- --parallel 2
```

Or directly:

```bash
bash batch/batch-runner.sh --parallel 2
```

The runner uses `codex exec` workers, logs progress in `batch/batch-state.tsv`, writes tracker additions to `batch/tracker-additions/`, then merges them with `node merge-tracker.mjs`.

## Recurring Scans

There is no repo slash-command scheduler. Use a real Codex-native rerun path:

- Codex app automation, if your Codex surface supports automations
- cron or another scheduler running `codex exec`

Example cron-style command:

```bash
cd /path/to/career-ops && codex exec --dangerously-bypass-approvals-and-sandbox --search "Use the career-ops-scan skill. Read AGENTS.md, scan portals.yml, update data/scan-history.tsv and data/pipeline.md, then summarize the new roles."
```

Optional convenience prompt files are installed under `~/.codex/prompts/` for the common workflows. The repo does not depend on them.

## User Data vs System Files

User data and personalization live here:
- `cv.md`
- `config/profile.yml`
- `modes/_profile.md`
- `article-digest.md`
- `portals.yml`
- `data/`
- `reports/`
- `output/`
- `interview-prep/`

System/runtime files live here:
- `AGENTS.md`
- `.agents/skills/`
- `modes/`
- `templates/`
- `batch/`
- `*.mjs`
- `dashboard/`

User-specific customization belongs in `config/profile.yml` or `modes/_profile.md`, not in `modes/_shared.md`.

## Customize The System

Common customizations:
- update your identity, target roles, narrative, comp targets, or constraints in `config/profile.yml`
- store user-specific archetypes, negotiation preferences, and proof-point framing in `modes/_profile.md`
- add or remove tracked companies and queries in `portals.yml`
- change the CV look in `templates/cv-template.html`
- change repo-wide scoring or prompts in `modes/_shared.md`, the relevant mode file, or `batch/batch-prompt.md`

The system is designed to be edited by Codex itself. Ask for the change in plain language and Codex can patch the correct file.

## Language Modes

Default runtime files live in `modes/`. Additional translated mode directories include:
- `modes/de/`
- `modes/fr/`

Set `language.modes_dir` in `config/profile.yml` to prefer one of those directories. If a translated file does not exist for a workflow, Career-Ops falls back to the default file under `modes/`.

## Scanner Coverage

The scanner ships with 45+ pre-configured companies and 19 search queries across major boards and ATS systems.

Examples:
- AI labs: Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
- Voice AI: ElevenLabs, PolyAI, Deepgram, Vapi, Bland AI
- AI platforms: Retool, Airtable, Vercel, Temporal, Glean, Arize AI
- Automation: n8n, Zapier, Make.com
- Job boards: Ashby, Greenhouse, Lever, Wellfound, Workable, RemoteFront

## Dashboard TUI

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard
```

## Project Structure

```text
career-ops/
├── AGENTS.md
├── .agents/skills/
├── cv.md
├── article-digest.md
├── config/
├── modes/
├── templates/
├── batch/
├── dashboard/
├── data/
├── reports/
├── output/
├── fonts/
├── docs/
└── examples/
```

## Also Open Source

- [cv-santiago](https://github.com/santifer/cv-santiago) — the portfolio site that accompanies this system

## Author

Santiago Fernández de Valderrama built Career-Ops to manage his own search and used it to land his current role. More at [santifer.io](https://santifer.io).

## License

MIT
