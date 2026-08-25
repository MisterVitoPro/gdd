# Repository guidance

- Maintain Game Doc Forge (`gdd`) as a dual-client plugin for Claude Code and Codex.
- Keep `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` on the same semantic version.
- Keep skill frontmatter compatible with Codex: only supported fields, and each skill name must match its folder name.
- Keep orchestration host-neutral. Document Claude slash commands (`/gdd:create`) and Codex `$` skill mentions at user-facing invocation points.
- Resolve bundled agent role files and templates relative to the active `SKILL.md`; Codex does not register files under `agents/` as named agents automatically.
- `concept-gatherer`, `interview-video-game`, and `interview-tabletop` are interactive and must run inline in the main session; every other role is dispatched as a subagent and must never ask the user questions directly.
- The three interactive roles share `templates/interview_protocol.md` (mechanics, depth levels, ledger format). Put question content in the role files and method changes in the protocol; do not duplicate one into the other.
- The interview forks on `gameInfo.family` (`video` | `tabletop`) set by `concept-gatherer`. Board, card, dice, party, miniatures, and TTRPG designs all use `interview-tabletop`, which branches internally by `gameInfo.type`.
- `interview-ledger.md` is the traceability source: `gdd-writer` must not state anything normative that lacks a `decided` or `assumed` row, and `gdd-auditor` enforces that. New question modules must emit ledger rows with the `VG-`/`TT-`/`CG-` ID scheme.
- GDD structure lives in `templates/gdd_template_video.md` and `templates/gdd_template_tabletop.md`. Change a section in the template, the writer's section guide, and the auditor's checklist together.
- `docs/gdd-best-practices.md` records the research behind the template and interview design, with sources. Update it when the rationale changes; the writer and auditor read it.
- Do not add a `commands/` directory: Claude Code registers every Markdown file there as a slash command. The command reference lives in `docs/commands.md`.
- Runtime output always lands in `.gdd/sessions/<project>/` under the user's working directory; the plugin never writes elsewhere.
- Stage names are fixed in `skills/create/SKILL.md` and checked by CI. `DEEP_DIVE` was replaced by `INTERVIEW` in 2.0; `resume` migrates 1.0 sessions.
- No emojis in any file.
- Run `claude plugin validate . --strict` and the workflow checks in `.github/workflows/validate.yml` before releasing.
- Release with an immutable plain `v<version>` tag. The plugin is published through the `game-dev-2d` marketplace (https://github.com/MisterVitoPro/game-dev-2d), which registers it by `github` source tracking `main`, so bump the version in both manifests before merging to `main`; the README version badges read `.claude-plugin/plugin.json` live.
