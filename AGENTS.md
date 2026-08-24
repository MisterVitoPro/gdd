# Repository guidance

- Maintain Game Doc Forge (`gdd`) as a dual-client plugin for Claude Code and Codex.
- Keep `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` on the same semantic version.
- Keep skill frontmatter compatible with Codex: only supported fields, and each skill name must match its folder name.
- Keep orchestration host-neutral. Document Claude slash commands (`/gdd:create`) and Codex `$` skill mentions at user-facing invocation points.
- Resolve bundled agent role files and templates relative to the active `SKILL.md`; Codex does not register files under `agents/` as named agents automatically.
- `concept-gatherer` and `deep-dive` are interactive and must run inline in the main session; every other role is dispatched as a subagent and must never ask the user questions directly.
- Do not add a `commands/` directory: Claude Code registers every Markdown file there as a slash command. The command reference lives in `docs/commands.md`.
- Runtime output always lands in `.gdd/sessions/<project>/` under the user's working directory; the plugin never writes elsewhere.
- No emojis in any file.
- Run `claude plugin validate . --strict` and the workflow checks in `.github/workflows/validate.yml` before releasing.
- Release with an immutable plain `v<version>` tag, then update the catalog in `qa-claude-market` to that tag and commit SHA.
