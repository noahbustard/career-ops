# Customization Guide

Career-Ops is designed to be customized in-place. The important distinction is where to put personal data versus repo-wide behavior.

## User-Specific Customization

Put personal customization in:
- `config/profile.yml`
- `modes/_profile.md`
- `article-digest.md`
- `portals.yml`

Use those files for:
- name, email, location, timezone, compensation targets
- target roles and deal-breakers
- proof points, narrative, exit story, positioning
- user-specific archetypes and framing
- tracked companies and search keywords

Do not put user-specific content into `modes/_shared.md`.

## Repo-Wide Customization

Use repo-wide files only when you want to change the system for every user of the repo:
- `modes/_shared.md`
- the individual mode files under `modes/`
- `batch/batch-prompt.md`
- `templates/cv-template.html`
- `templates/states.yml`

## Profile (`config/profile.yml`)

This is the source of truth for identity and search intent.

Typical sections:
- candidate identity and links
- target roles
- narrative and proof points
- compensation
- location and work authorization
- language preferences such as `language.modes_dir`

## Personal Framing (`modes/_profile.md`)

Use this file for user-specific prompting that should survive repo updates:
- your own archetype tweaks
- project framing
- interview positioning
- negotiation preferences
- things to emphasize or avoid

## Portals (`portals.yml`)

Customize:
1. `title_filter.positive`
2. `title_filter.negative`
3. `search_queries`
4. `tracked_companies`

## CV Template (`templates/cv-template.html`)

The default PDF design uses:
- Space Grotesk for headings
- DM Sans for body text
- a single-column ATS-friendly layout

Edit the template CSS or font files if you want a different visual style.

## Canonical States (`templates/states.yml`)

If you change tracker states, update all consumers together:
1. `templates/states.yml`
2. `normalize-statuses.mjs`
3. `merge-tracker.mjs`
4. any mode text that references statuses

## Codex Runtime Files

The Codex-facing runtime layer lives in:
- `AGENTS.md`
- `.agents/skills/`

These files should stay focused on discovery and execution. Personalization belongs in the user-layer files above.
