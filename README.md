# Game Doc Forge (`gdd`)

A Claude Code and Codex plugin that turns a game idea into a Game Design Document a team can build from. The pipeline runs a pillar-first concept interview, forks into a comprehensive video-game or tabletop interview, optionally researches the web, writes the GDD from a family-specific template, audits it for traceability and completeness, generates supplementary documents, and builds a session index. Works for video games, board games, card games, dice and party games, miniatures games, and tabletop RPGs.

Part of the [MisterVitoPro Plugin Marketplace](https://github.com/MisterVitoPro/qa-claude-market).

## What is different about 2.0

Version 2.0 rebuilt the plugin around research into what makes design documents useful (Librande's one-page designs, Ryan's anatomy of a design document, design pillars, MDA and experience goals, the Ludology and Stonemaier tabletop checklists, accessibility and localization guidelines; see [docs/gdd-best-practices.md](docs/gdd-best-practices.md) for the sourced summary).

- **Pillars and experience goals before mechanics.** The concept interview will not accept a pillar the designer cannot name a cut for.
- **A forked deep interview.** Video games get fourteen modules (vision and scope, platform and business, core loop and verbs, 3Cs and game feel, systems and economy, difficulty and onboarding, content and levels, narrative, art and audio, UI, multiplayer, accessibility and localization, technical foundations, production and risk) plus genre probes. Tabletop games get sixteen (vitals, theme and role, mechanisms and turn structure, decision space and interaction, economy and scoring, balance and luck, player-count scaling, cards and decks, components and production, rules and teach, graphic design and accessibility, prototyping and playtesting, publishing and pricing, campaign and legacy, and two TTRPG modules) plus genre probes.
- **Selectable depth.** Quick (20-30 questions), standard (45-70), or comprehensive (90+). Modules that do not apply are skipped; everything not asked becomes an explicit open question.
- **A ledger, not a transcript.** Every question gets a status: decided, assumed (with rationale), open, skipped, or superseded. The GDD writer may not state anything the ledger does not support, and an auditor checks.
- **Family-specific templates.** The video template is built on the union of the major GDD templates with sections tiered Core, Recommended, and Optional. The tabletop template covers sell-sheet vitals, component manifest, cost sanity, rulebook and teach, prototype and playtest logs, and publishing.
- **Living-document appendices.** Decision log, assumptions, open questions with owners, glossary, document history.
- **Research that targets open questions.** The research planner reads the ledger and prioritizes topics that can close open items.

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
| `/gdd:resume [project]` | Continue an interrupted session, answer open questions, extend the interview, or revise |
| `/gdd:status [project]` | Read-only progress report for a session (or list all sessions) |

Full reference, checkpoint prompts, and state schema: [docs/commands.md](docs/commands.md).

## Pipeline

```
/gdd:create
    |
    v
[1. Concept Gathering]  (inline) ------> concept.md, ledger CG rows
    |   family? pitch, pillars, fantasy, audience, exclusions, constraints
    |
    +----------- video ----------+----------- tabletop -----------+
    |                                                             |
    v                                                             v
[2. Video Game Interview] (inline)               [2. Tabletop Interview] (inline)
    depth checkpoint, 14 modules + genre probes      depth checkpoint, 16 modules + genre probes
    |                                                             |
    +-------------------------------------------------------------+
    |
    v ----------------------------------------> interview-ledger.md, details.md
[CHECKPOINT: research or skip?]
    |
    +--- skip ---------------------------------------------+
    |                                                      |
    v                                                      |
[3. Research Planning] ------------------> research-plan.md
    |   (targets open ledger items first)                  |
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
[6. GDD Writing] ------------------------> GDD.md (video or tabletop template)
    |
    v
[7. GDD Audit] --------------------------> gdd-audit.md (blocking findings send the writer back once)
    |
    v
[CHECKPOINT: review GDD / answer open questions / revise]
    |
    v
[8. Supplement Analysis] ----------------> supplement-plan.md
    |
    v
[CHECKPOINT: select supplements]
    |
    v
[9. Supplement Generation] (parallel) ---> supplements/*.md
    |
    v
[10. Index Generation] ------------------> INDEX.md
```

## Output

Everything lands in `.gdd/sessions/<project>/` under your working directory:

```
.gdd/sessions/my-game/
  INDEX.md                # read this first in later sessions
  state.json              # pipeline state for resume
  concept.md              # pitch, pillars, audience, exclusions, constraints
  interview-ledger.md     # every question, answer, and status with stable IDs
  details.md              # synthesized design details, assumptions, open questions
  research-plan.md        # if research enabled
  research/               # if research enabled
    historical.md
    market.md
    technical.md
  research_synthesis.md   # if research enabled
  GDD.md
  gdd-audit.md            # auditor findings on the final draft
  supplement-plan.md
  supplements/
    stats_combat.md
    playtest_plan.md
    sell_sheet.md
    ...
```

Add `.gdd/` to your project's `.gitignore` if you do not want sessions committed.

## GDD structure

**Video game** (`templates/gdd_template_video.md`)

1. Vision (one page): pitch, pillars with cuts, what it is not, fantasy and experience goals, audience, comparables, USPs, features, scope and MVP, game flow
2. Core Gameplay: loop at four timescales, verbs, tension and choice, objectives and failure, feedback, randomness
3. Character, Camera, Controls, and Game Feel
4. Systems: inventory with pillar mapping, one subsection per system, AI
5. Economy and Progression
6. Difficulty, Onboarding, and Retention
7. Content and Level Design
8. Narrative, Characters, and World
9. Art, Audio, and Presentation
10. UI and UX
11. Multiplayer, Online, and Social
12. Accessibility and Localization
13. Technical Foundations
14. Production, Business, and Risk
15. Market Positioning (optional)
Appendices: decision log, assumptions, open questions, glossary, research notes, document history

**Tabletop** (`templates/gdd_template_tabletop.md`)

1. Vision (one page): pitch, pillars, what it is not, core experience, audience and weight, shelf neighbours, hooks, scope and MVP prototype
2. Theme and Player Role
3. Mechanisms and Turn Structure
4. Decision Space and Interaction
5. Economy, Scoring, and End Game
6. Balance, Pacing, and Luck
7. Player-Count Scaling and Solo
8. Cards and Decks
9. Components, Table, and Production
10. Rules, Teach, and Reference
11. Graphic Design and Accessibility
12. Prototyping and Playtesting
13. Publishing, Pricing, and Market
14. TTRPG: Resolution and Characters
15. TTRPG: GM Tools, Setting, and Campaign
16. Campaign, Legacy, and Expansion (optional)
Appendices: as above

## Plugin layout

```
gdd/
  .claude-plugin/plugin.json    # Claude Code manifest
  .codex-plugin/plugin.json     # Codex manifest
  skills/
    create/SKILL.md             # orchestrator: the full pipeline
    resume/SKILL.md             # resume, answer open questions, extend, revise
    status/SKILL.md             # read-only progress report
  agents/                       # role prompts, loaded relative to SKILL.md
    concept-gatherer.md         # inline; decides the fork
    interview-video-game.md     # inline; video branch
    interview-tabletop.md       # inline; tabletop branch
    research-planner.md
    historical-researcher.md
    market-researcher.md
    technical-researcher.md
    research-vetter.md
    gdd-writer.md
    gdd-auditor.md
    supplement-analyzer.md
    supplement-writer.md
    index-generator.md
  templates/
    interview_protocol.md       # shared interview method, depth levels, ledger format
    gdd_template_video.md
    gdd_template_tabletop.md
    supplement_templates.md
    state_template.json
  docs/
    commands.md                 # command and checkpoint reference
    gdd-best-practices.md       # the research behind the design, with sources
```

The three interview roles run inline in the main session because they talk to the user; every other role is dispatched as a subagent. In Claude Code the role files are also registered as `gdd:<role>` agent types, but the skills never depend on that so Codex works from the same files.

## Customization

- **Interview questions**: edit the module in `agents/interview-video-game.md` or `agents/interview-tabletop.md`; keep the `[Q]`/`[S]`/`[C]` tags and the exit checks
- **Interview method**: edit `templates/interview_protocol.md`
- **GDD structure**: edit the family template, then the matching section guide in `agents/gdd-writer.md` and the coverage checklist in `docs/gdd-best-practices.md`
- **New supplement types**: add a template to `templates/supplement_templates.md` and teach `agents/supplement-analyzer.md` to recommend it

## Upgrading from 1.0

Sessions created with 1.0 (`state.json` version `1.0`) are migrated by `/gdd:resume`: the `DEEP_DIVE` stage becomes `INTERVIEW`, new state keys are added, and you are offered the chance to run the new interview before writing, since 1.0 sessions have no ledger for the auditor to trace against.

## Requirements

- Claude Code or Codex
- Web search available to subagents if you want the research phase

## License

MIT
