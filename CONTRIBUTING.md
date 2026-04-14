# Contributing to Career-Ops

Thanks for contributing. Career-Ops is now documented and operated as a Codex-first repo.

## Before Opening a PR

Open an issue first so the change can be discussed before implementation. PRs that bypass that step may be closed if they do not fit the repo’s direction.

## Good PRs

- fix an agreed bug
- improve Codex-first docs or setup
- add or improve translated modes
- improve the scanner, batch runner, or dashboard without changing the repo’s core quality-over-quantity philosophy
- keep changes minimal and well explained

## Development Flow

1. open an issue
2. fork the repo
3. create a branch
4. make the change
5. test with a fresh clone and a Codex-based setup
6. open a PR that references the issue

## Guidelines

- keep user data out of commits (`cv.md`, `config/profile.yml`, `modes/_profile.md`, `data/`, `reports/`, `output/`)
- keep user-specific behavior in the profile layer, not in `modes/_shared.md`
- keep docs, `AGENTS.md`, and `.agents/skills/` aligned
- scripts should handle missing files gracefully
- dashboard changes should be validated with `go build`

## Useful Commands

```bash
npm run doctor
node cv-sync-check.mjs
node verify-pipeline.mjs
node merge-tracker.mjs
bash batch/batch-runner.sh --help
```

## Need Help?

- [Open an issue](https://github.com/santifer/career-ops/issues)
- [Read the setup guide](docs/SETUP.md)
- [Read the architecture guide](docs/ARCHITECTURE.md)
