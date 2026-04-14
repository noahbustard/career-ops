# Career-Ops -- Codex Operating Guide

Career-Ops is an AI-assisted job-search pipeline for evaluating offers, generating tailored CV PDFs, scanning portals, and tracking applications. The repo is Codex-first: use plain-language requests or the repo skills under `.agents/skills/`, not slash commands.

## Codex Entry Points

Main repo skills:
- `career-ops` — general router and discovery
- `career-ops-auto-pipeline` — full job evaluation from JD text or URL
- `career-ops-scan` — portal scanning into `data/pipeline.md`
- `career-ops-pipeline` — process pending URLs from `data/pipeline.md`
- `career-ops-pdf` — generate an ATS-optimized CV PDF
- `career-ops-tracker` — read or update the tracker
- `career-ops-batch` — review or run the batch workflow

Secondary modes still exist and should be routed from plain-language requests:
- `oferta`, `ofertas`, `contacto`, `deep`, `training`, `project`, `apply`

Mode-loading rules:
1. Read `AGENTS.md`.
2. Read `config/profile.yml` if it exists.
3. Read `modes/_profile.md` if it exists.
4. Resolve `language.modes_dir` from `config/profile.yml` when present.
5. Use translated files from that directory when they exist; otherwise fall back to `modes/`.
6. For `auto-pipeline`, `oferta`, `ofertas`, `contacto`, `apply`, `pipeline`, `scan`, `batch`, and `pdf`, load `_shared.md` plus the mode file.
7. For `tracker`, `deep`, `training`, and `project`, load only the mode file.

Use subagents only when they materially help, especially for `scan`, `apply`, or `pipeline` with many URLs.

## Data Contract

Read `DATA_CONTRACT.md` for the full list.

User layer (never auto-updated, personalization goes here):
- `cv.md`
- `config/profile.yml`
- `modes/_profile.md`
- `article-digest.md`
- `portals.yml`
- `data/*`
- `reports/*`
- `output/*`
- `interview-prep/*`

System layer (auto-updatable, do not store user data here):
- `modes/_shared.md`
- mode files under `modes/`
- `AGENTS.md`
- `*.mjs` scripts
- `dashboard/*`
- `templates/*`
- `batch/*`

Critical rule:
- User-specific customization always goes to `modes/_profile.md` or `config/profile.yml`.
- Do not put user-specific archetypes, negotiation preferences, narrative, proof points, location policy, or compensation targets into `modes/_shared.md`.

## Update Check

On the first message of each session, run this silently:

```bash
node update-system.mjs check
```

Interpret the JSON response:
- `update-available`: tell the user `career-ops update available (v{local} → v{remote}). Your data (CV, profile, tracker, reports) will NOT be touched. Want me to update?`
- If yes, run `node update-system.mjs apply`
- If no, run `node update-system.mjs dismiss`
- `up-to-date`, `dismissed`, or `offline`: say nothing

The user can also ask to check for updates or rollback with `node update-system.mjs rollback`.

## Session Bootstrap

Before evaluations, scans, or other workflows, check:
1. `cv.md`
2. `config/profile.yml`
3. `modes/_profile.md`
4. `portals.yml`

If `modes/_profile.md` is missing, copy `modes/_profile.template.md` to `modes/_profile.md` silently.

If any required setup file is missing, enter onboarding mode before proceeding.

### Onboarding Step 1 — CV

If `cv.md` is missing, ask:

> I don't have your CV yet. You can either:
> 1. Paste your CV here and I'll convert it to markdown
> 2. Paste your LinkedIn URL and I'll extract the key info
> 3. Tell me about your experience and I'll draft a CV for you
>
> Which do you prefer?

Create `cv.md` with standard markdown sections: Summary, Experience, Projects, Education, Skills.

### Onboarding Step 2 — Profile

If `config/profile.yml` is missing, copy `config/profile.example.yml` first, then ask for:
- full name and email
- location and timezone
- target roles
- salary target range

Write those answers to `config/profile.yml`. Store user-specific narrative, targeting, and deal-breakers here or in `modes/_profile.md`.

### Onboarding Step 3 — Portals

If `portals.yml` is missing:
- copy `templates/portals.example.yml` to `portals.yml`
- ask whether to customize the search keywords and tracked companies
- update `title_filter.positive` to reflect the target roles already given

### Onboarding Step 4 — Tracker

If `data/applications.md` does not exist, create:

```markdown
# Applications Tracker

| # | Date | Company | Role | Score | Status | PDF | Report | Notes |
|---|------|---------|------|-------|--------|-----|--------|-------|
```

### Onboarding Step 5 — Learn the Candidate

After the basics exist, proactively ask for:
- unique strengths
- energizers vs drainers
- deal-breakers
- best professional achievement
- articles, projects, or case studies

Store proof points in `article-digest.md` and user-specific positioning in `config/profile.yml` or `modes/_profile.md`.

After every evaluation, learn from user feedback and update the profile layer accordingly.

### Onboarding Step 6 — Ready

Once the basics exist, confirm that the user can now:
- paste a job URL or JD for full evaluation
- ask Codex to scan portals
- process the pipeline inbox
- generate a tailored PDF
- inspect or update the tracker

For recurring scans, prefer real Codex-native options:
- Codex app automation, if available in the user’s Codex surface
- a cron job that runs `codex exec` with the scan skill

## Personalization

This system is meant to be customized directly in the repo.

Common requests:
- change the candidate archetypes or positioning for a specific person → edit `modes/_profile.md`
- update identity, target roles, narrative, compensation, or constraints → edit `config/profile.yml`
- add or remove tracked companies and search filters → edit `portals.yml`
- adjust the CV design → edit `templates/cv-template.html`
- change repo-wide scoring or mode logic for everyone → edit `modes/_shared.md` and the relevant mode files

## Language Modes

Default shared runtime is `modes/`. Additional translated mode directories exist, including:
- `modes/de/`
- `modes/fr/`

Use translated modes when:
- the user asks for that language
- the target market is clearly localized
- `config/profile.yml` sets `language.modes_dir`

If a translated file is missing for the requested workflow, fall back to the default file under `modes/`.

## CV Source of Truth

- `cv.md` is the canonical CV
- `article-digest.md` stores richer proof points
- never hardcode metrics when evaluating or generating documents

## Ethical Use

This system is for quality, not quantity.

- Never submit an application without explicit user review
- Strongly discourage low-fit roles below `4.0/5`
- Favor a few strong applications over mass outreach
- Respect recruiter attention and candidate time

## Offer Verification

Never trust web search or raw fetch alone to verify whether an offer is still active.

Use Playwright whenever the runtime supports it:
1. navigate to the URL
2. inspect the rendered page
3. treat title + description + apply action as active
4. treat footer/navbar-only pages or clear closed-job copy as closed

Headless batch workers do not have the same interactive browser affordances. When the batch runner cannot fully verify liveness, mark the report header as:

```markdown
**Verification:** unconfirmed (batch mode)
```

## Stack and Conventions

- Node.js `.mjs` scripts
- Playwright for PDF generation and browser-driven job parsing
- YAML config
- Markdown data
- HTML/CSS CV template

Repo conventions:
- outputs go to `output/`
- reports go to `reports/`
- local JDs can live in `jds/` and be referenced as `local:jds/{file}`
- batch files live in `batch/`
- report numbering is sequential, three digits, `max(existing) + 1`

## Tracker Additions

Never add new tracker rows directly to `applications.md`.

Write one TSV line per evaluation to:

```text
batch/tracker-additions/{num}-{company-slug}.tsv
```

Column order:
1. `num`
2. `date`
3. `company`
4. `role`
5. `status`
6. `score`
7. `pdf`
8. `report`
9. `notes`

Example:

```text
{num}\t{date}\t{company}\t{role}\t{status}\t{score}/5\t{pdf_emoji}\t[{num}](reports/{num}-{slug}-{date}.md)\t{note}
```

After each batch of evaluations, run:

```bash
node merge-tracker.mjs
```

## Pipeline Integrity

Rules:
1. Never add new rows directly to `applications.md`
2. Updating status or notes on an existing row is allowed
3. Every report must include `**URL:**` in the header
4. Status values must be canonical per `templates/states.yml`
5. Use these health scripts as needed:

```bash
node verify-pipeline.mjs
node normalize-statuses.mjs
node dedup-tracker.mjs
```

Canonical status labels are defined in `templates/states.yml`.
