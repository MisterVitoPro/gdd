# Game Doc Forge (`gdd`)

A Claude Code and Codex plugin that turns a game idea into a comprehensive Game Design Document through a multi-agent pipeline: guided concept Q&A, optional parallel web research, a 15-section GDD, supplementary documents, and a session index. Works for video games, board games, card games, and tabletop RPGs.

Part of the [MisterVitoPro Plugin Marketplace](https://github.com/MisterVitoPro/qa-claude-market).

- **Guided discovery**: structured concept gathering and game-type-aware deep-dive questions
- **Optional research**: historical/domain, market, and technical researchers run in parallel, then a vetter synthesizes findings
- **15-section GDD**: from executive summary through post-launch roadmap, adapted to the game type
- **Supplements**: stats tables, item lists, ability matrices, character sheets, lore bible, rules references, and more, generated in parallel
- **Checkpoints**: research decision, topic selection, GDD review, supplement selection
- **Resumable**: every session persists `state.json` for `/gdd:resume`
- **INDEX.md**: a catalog of every generated file so future sessions know where to look

## Quick start

```bash
# Claude Code
claude plugin marketplace add MisterVitoPro/qa-claude-market
claude plugin install gdd@esper

# Local development: clone this repo and point the plugin loader at it
git clone https://github.com/MisterVitoPro/gdd.git
claude --plugin-dir ./gdd
```

Then, inside a project:

```
/gdd:create my-game        # Claude Code
$gdd:create my-game        # Codex
```

## Commands

| Command | Purpose |
|---------|---------|
| `/gdd:create [project]` | Run the full pipeline for a new project |
| `/gdd:resume [project]` | Continue an interrupted or paused session from its last stage |
| `/gdd:status [project]` | Read-only progress report for a session (or list all sessions) |

Full reference, checkpoint prompts, and state schema: [docs/commands.md](docs/commands.md).

## Pipeline

```
/gdd:create
    |
    v
[1. Concept Gathering]  (inline Q&A) ----> concept.md
    |
    v
[2. Deep Dive]          (inline Q&A) ----> details.md
    |
    v
[CHECKPOINT: research or skip?]
    |
    +--- skip ---------------------------------------------+
    |                                                      |
    v                                                      |
[3. Research Planning] ------------------> research-plan.md
    |                                                      |
    v                                                      |
[CHECKPOINT: select topics]                                |
    |                                                      |
    v                                                      |
[4. Research Execution] (parallel) ------> research/*.md   |
    |                                                      |
    v                                                      |
[5. Research Vetting] -------------------> research_synthesis.md
    |                                                      |
    v <----------------------------------------------------+
[6. GDD Writing] ------------------------> GDD.md
    |
    v
[CHECKPOINT: review GDD]
    |
    v
[7. Supplement Analysis] ----------------> supplement-plan.md
    |
    v
[CHECKPOINT: select supplements]
    |
    v
[8. Supplement Generation] (parallel) ---> supplements/*.md
    |
    v
[9. Index Generation] -------------------> INDEX.md
```

## Output

Everything lands in `.gdd/sessions/<project>/` under your working directory:

```
.gdd/sessions/my-game/
  INDEX.md                # read this first in later sessions
  state.json              # pipeline state for resume
  concept.md
  details.md
  research-plan.md        # if research enabled
  research/               # if research enabled
    historical.md
    market.md
    technical.md
  research_synthesis.md   # if research enabled
  GDD.md
  supplement-plan.md
  supplements/
    stats_combat.md
    items_weapons.md
    lore_bible.md
    ...
```

Add `.gdd/` to your project's `.gitignore` if you do not want sessions committed.

## GDD sections

1. Executive Summary
2. Game Concept
3. Core Mechanics
4. Gameplay Systems
5. Progression Systems
6. Story and Narrative
7. Characters
8. World Design
9. Visual Style
10. Audio Design (video) / Components (tabletop)
11. User Interface
12. Technical Requirements (video) / Rules Reference (tabletop)
13. Monetization Strategy
14. Marketing Positioning
15. Post-Launch Roadmap

## Plugin layout

```
gdd/
  .claude-plugin/plugin.json    # Claude Code manifest
  .codex-plugin/plugin.json     # Codex manifest
  skills/
    create/SKILL.md             # orchestrator: the full pipeline
    resume/SKILL.md             # resume from state.json
    status/SKILL.md             # read-only progress report
  agents/                       # role prompts, loaded relative to SKILL.md
    concept-gatherer.md         # inline
    deep-dive.md                # inline
    research-planner.md
    historical-researcher.md
    market-researcher.md
    technical-researcher.md
    research-vetter.md
    gdd-writer.md
    supplement-analyzer.md
    supplement-writer.md
    index-generator.md
  templates/
    gdd_template.md
    supplement_templates.md
    state_template.json
  docs/commands.md              # command and checkpoint reference
```

`concept-gatherer` and `deep-dive` run inline in the main session because they talk to the user; every other role is dispatched as a subagent. In Claude Code the role files are also registered as `gdd:<role>` agent types, but the skills never depend on that so Codex works from the same files.

## Customization

- **Agent behavior**: edit the role file in `agents/`
- **GDD structure**: edit `templates/gdd_template.md`
- **New supplement types**: add a template to `templates/supplement_templates.md` and teach `agents/supplement-analyzer.md` to recommend it

## Requirements

- Claude Code or Codex
- Web search available to subagents if you want the research phase

## License

MIT
