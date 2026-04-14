# Local Codex Prompt Files

Career-Ops installs optional convenience prompt files under `~/.codex/prompts/`.

Installed names:
- `career-ops.md`
- `career-ops-scan.md`
- `career-ops-pipeline.md`
- `career-ops-pdf.md`
- `career-ops-tracker.md`
- `career-ops-batch.md`

These files are convenience wrappers only. The repo does not depend on them; the primary runtime remains `AGENTS.md` plus the repo skills under `.agents/skills/`.

Suggested usage:
- use the matching prompt file for a fast starting prompt in Codex, if your Codex surface exposes local prompt files
- otherwise paste the same instructions manually or call `codex -C /path/to/career-ops "..."`
