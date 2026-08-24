---
name: create
description: Generate a comprehensive Game Design Document through a multi-agent pipeline. Use when the user wants to create a GDD, design a game, write game documentation, or plan out a video game, board game, card game, or tabletop RPG. Runs a concept interview, forks into a video-game or tabletop deep interview, optionally researches the web, writes and audits the GDD, generates supplements, and builds an index, with user checkpoints throughout.
---

# gdd:create - Game Design Document pipeline

Run the full GDD pipeline for one project. Treat the text supplied with the skill invocation as the project name; if none was supplied, ask for one before doing anything else. Normalize the name to a filesystem-safe slug (lowercase, hyphens) and confirm it with the user.

## Portable role loading

Resolve bundled files relative to this `SKILL.md`, not the current working directory. From this file:

- role prompts live in `../../agents/<role>.md`
- document templates live in `../../templates/`
- the interview method lives in `../../templates/interview_protocol.md`
- the design principles the writer and auditor apply live in `../../docs/gdd-best-practices.md`
- the command reference lives in `../../docs/commands.md`

Compute the absolute plugin root once at the start of the run and reuse it. Before running or dispatching any role, read that role's Markdown file. Claude Code may also expose the same files as namespaced agent types (`gdd:<role>`), but never depend on that registration: Codex discovers the skills and does not automatically register `agents/` files.

## Interactive versus dispatched roles

Subagents cannot talk to the user, so roles split two ways:

| Role | Mode | Why |
|------|------|-----|
| `concept-gatherer` | **Inline** - read the role file and follow it yourself in the main session | Interactive Q&A; decides the fork |
| `interview-video-game` | **Inline** | Interactive Q&A (video game branch) |
| `interview-tabletop` | **Inline** | Interactive Q&A (board, card, dice, TTRPG branch) |
| `research-planner` | Dispatch as subagent | Pure analysis |
| `historical-researcher`, `market-researcher`, `technical-researcher` | Dispatch as subagents, in parallel | Web research |
| `research-vetter` | Dispatch as subagent | Synthesis |
| `gdd-writer` | Dispatch as subagent | Long-form writing |
| `gdd-auditor` | Dispatch as subagent | Traceability and completeness audit |
| `supplement-analyzer` | Dispatch as subagent | Analysis |
| `supplement-writer` | Dispatch as subagents, one per selected supplement, in parallel | Long-form writing |
| `index-generator` | Dispatch as subagent | Cataloging |

Use the host's native subagent facility for dispatched roles. Parallelize independent roles in one batch when the host supports it. Use an available structured-input facility (`AskUserQuestion` in Claude Code, `request_user_input` in Codex) for checkpoints and inline Q&A; when none exists, ask the same numbered options in concise prose and wait for the answer. The inline roles all follow `templates/interview_protocol.md`; read it once before Stage 1.

## Session directory and state

All output goes to `.gdd/sessions/<project>/` under the current working directory:

```
.gdd/sessions/<project>/
  INDEX.md                # file catalog for future sessions (written last)
  state.json              # pipeline state for resume
  concept.md              # Stage 1
  interview-ledger.md     # Stages 1-2: every question, answer, and status
  details.md              # Stage 2: synthesized design details
  research-plan.md        # if research enabled
  research/               # if research enabled
    historical.md
    market.md
    technical.md
  research_synthesis.md   # if research enabled
  GDD.md
  gdd-audit.md            # auditor findings for the review checkpoint
  supplement-plan.md
  supplements/*.md
```

Create the directory and initialize `state.json` from `../../templates/state_template.json` before Stage 1 (fill `projectName`, `createdAt`, `updatedAt`). If a session already exists for this name, stop and offer `/gdd:resume <project>` instead of overwriting.

**Always update `state.json`:** before starting a stage, after completing a stage, after each interview module, when the user makes a checkpoint decision, and on any error. Write ISO timestamps to `updatedAt`.

Stage names, in order: `NOT_STARTED`, `CONCEPT_GATHERING`, `INTERVIEW`, `RESEARCH_DECISION`, `RESEARCH_PLANNING`, `RESEARCH_APPROVAL`, `RESEARCH_EXECUTION`, `RESEARCH_VETTING`, `GDD_WRITING`, `GDD_AUDIT`, `GDD_REVIEW`, `SUPPLEMENT_ANALYSIS`, `SUPPLEMENT_SELECTION`, `SUPPLEMENT_GENERATION`, `INDEX_GENERATION`, `COMPLETED`.

## Pipeline

Execute stages in this order. Do not skip checkpoints.

### 1. CONCEPT_GATHERING (inline)
- Read `agents/concept-gatherer.md` and follow it. Game family first: it decides the fork.
- Output: `concept.md`, first rows of `interview-ledger.md`, `gameInfo.*` and `interview.role` in `state.json`.
- Next: INTERVIEW

### 2. INTERVIEW (inline, forked)
- Read the role named in `state.json` `interview.role`: `agents/interview-video-game.md` when `gameInfo.family` is `video`, `agents/interview-tabletop.md` when it is `tabletop`.
- The first thing the role does is the **depth checkpoint**: quick / standard / comprehensive (defined in the protocol). Record it in `checkpoints.interviewDepth` and `interview.depth`, then plan the module list from the role's triggers and write it to `interview.modulesPlanned`.
- Run the modules in order, appending to `interview-ledger.md` and `interview.modulesCompleted` after each one. Questions must be specific to the genre, theme, comparables, and pillars captured in `concept.md`.
- Output: `interview-ledger.md` (complete), `details.md`, `interview.counts` in `state.json`.
- Next: RESEARCH_DECISION

### 3. RESEARCH_DECISION (checkpoint)
Summarize what research could cover for this game (historical/domain, market, technical - technical only for video games) and list the `open` ledger items research might help resolve. Ask:

1. Conduct research (recommended for real-world settings, specific platforms, crowded markets, or when several open items are factual questions)
2. Skip research and go straight to GDD writing

Record `checkpoints.researchDecision`. Conduct -> RESEARCH_PLANNING. Skip -> GDD_WRITING.

### 4. RESEARCH_PLANNING (dispatch)
- Role: `research-planner`. Input: `concept.md`, `details.md`, `interview-ledger.md`.
- Output: `research-plan.md`
- Next: RESEARCH_APPROVAL

### 5. RESEARCH_APPROVAL (checkpoint)
Present the topics from `research-plan.md` grouped by researcher with their priorities and which open ledger items each addresses. Let the user pick topics by number, `all`, `high`, or `skip`. Record `checkpoints.researchApproval.selectedTopics`. Skip -> GDD_WRITING; otherwise RESEARCH_EXECUTION.

### 6. RESEARCH_EXECUTION (dispatch, parallel)
Dispatch only the researchers that have selected topics, in one batch:
- `historical-researcher` -> `research/historical.md`
- `market-researcher` -> `research/market.md`
- `technical-researcher` -> `research/technical.md`

Each reads `concept.md`, `details.md`, and its topics from `research-plan.md`. Next: RESEARCH_VETTING.

### 7. RESEARCH_VETTING (dispatch)
- Role: `research-vetter`. Input: every file in `research/`, plus `interview-ledger.md` so it can mark which open items the research resolved.
- Output: `research_synthesis.md`
- Next: GDD_WRITING

### 8. GDD_WRITING (dispatch)
- Role: `gdd-writer`. Input: `concept.md`, `details.md`, `interview-ledger.md`, `research_synthesis.md` if present. Supply as `{template_path}` the absolute path of `templates/gdd_template_video.md` when `gameInfo.family` is `video`, otherwise `templates/gdd_template_tabletop.md`. Also supply the absolute path of `docs/gdd-best-practices.md`.
- Output: `GDD.md`
- Next: GDD_AUDIT

### 9. GDD_AUDIT (dispatch)
- Role: `gdd-auditor`. Input: `GDD.md`, `interview-ledger.md`, `concept.md`, `details.md`, the template used, and `docs/gdd-best-practices.md`.
- Output: `gdd-audit.md`. The auditor never edits `GDD.md`.
- If the audit reports any **blocking** findings (untraceable normative claims, missing core sections, pillar contradictions), re-dispatch `gdd-writer` in revision mode with the audit file and repeat the audit once. After the second audit, proceed to GDD_REVIEW regardless and surface what remains.
- Next: GDD_REVIEW

### 10. GDD_REVIEW (checkpoint)
Present: sections completed, approximate word count, the pillars as written, the three to five most consequential design decisions, the count of assumptions and open questions carried into the appendices, and the auditor's remaining findings (verbatim, short). Ask:

1. Approve and continue to supplement analysis
2. Request revisions (ask which sections and what to change, re-dispatch `gdd-writer` in revision mode with the feedback, re-run `gdd-auditor`, then return to this checkpoint; cap at 3 revision rounds before recommending approval and later manual edits)
3. Answer open questions now (walk the `open` ledger rows with the interview protocol, update the ledger and `details.md`, then revise the affected sections as in option 2)
4. Pause here (save state and stop; the user can `/gdd:resume` later)

Record `checkpoints.gddReview`.

### 11. SUPPLEMENT_ANALYSIS (dispatch)
- Role: `supplement-analyzer`. Input: `GDD.md`, `concept.md`, `details.md`.
- Output: `supplement-plan.md`
- Next: SUPPLEMENT_SELECTION

### 12. SUPPLEMENT_SELECTION (checkpoint)
Present the recommended supplements grouped by priority. Let the user pick by number, `recommended` (high priority), `all`, or `none`. Record `checkpoints.supplementSelection.selectedSupplements`. None -> INDEX_GENERATION; otherwise SUPPLEMENT_GENERATION.

### 13. SUPPLEMENT_GENERATION (dispatch, parallel)
Dispatch one `supplement-writer` per selected supplement in a single batch. Each gets `GDD.md`, its entry from `supplement-plan.md`, and the absolute path of `templates/supplement_templates.md`. Output: `supplements/<supplement_name>.md`. Record each written file in `stageOutputs.supplements`. Next: INDEX_GENERATION.

### 14. INDEX_GENERATION (dispatch)
- Role: `index-generator`. Input: every file in the session directory plus `state.json`.
- Output: `INDEX.md`
- Next: COMPLETED

### 15. COMPLETED
Set `currentStage` to `COMPLETED` and print the completion summary below.

## Dispatch prompt template

When dispatching a role, include the full body of its role file in the subagent prompt along with:

```
Current pipeline context:
- Project: {project_name}
- Stage: {current_stage}
- Game family / type: {gameInfo.family} / {gameInfo.type}
- Session path (absolute): {session_path}
- Plugin root (absolute): {plugin_root}
- Template path (absolute, when the role uses one): {template_path}
- Best-practices path (absolute, for gdd-writer and gdd-auditor): {best_practices_path}

Input files:
- {absolute paths of relevant inputs}

Execute the role's instructions and write the output to: {absolute output path}
Return a two-line summary: what was written and anything the orchestrator should surface to the user.
```

Never have a dispatched role ask the user questions; anything it cannot decide, it should note in its output for the orchestrator to raise at the next checkpoint.

## Checkpoint handling

At each checkpoint: save state with the checkpoint marked reached, present the information clearly, ask with the structured-input facility, validate the answer, record the decision in `state.json`, then proceed.

## Error handling

On any failure: append to `state.json` `errors` (stage, message, timestamp), tell the user what happened, and offer to retry the stage, skip to the next stage if that is safe, or save and exit. Never lose completed outputs. An interrupted interview is not an error: the ledger holds every completed module and `/gdd:resume` continues at the next one.

## Completion summary

```markdown
# GDD Generation Complete

## Project: {project_name}

### Generated documents
- INDEX.md - file index (read this first in future sessions)
- concept.md - pitch, pillars, audience, constraints
- interview-ledger.md - every design question with its status ({decided} decided, {assumed} assumed, {open} open)
- details.md - synthesized design details
- research_synthesis.md - consolidated research (if conducted)
- GDD.md - the Game Design Document
- gdd-audit.md - audit findings on the final draft
- supplements/ - {count} supplementary documents

### Session location
.gdd/sessions/{project_name}/

### What's next
1. Read GDD.md; the Open Questions appendix lists the decisions still owed
2. Prototype the riskiest pillar first (see the Risks section) and update the GDD from what you learn
3. Run `/gdd:resume {project_name}` to answer open questions, extend the interview, revise, or add supplements
```

## Key behaviors

1. Always ask rather than assume user intent; when a gap must be filled, label it an assumption.
2. Update state frequently so resume is reliable; the interview writes after every module.
3. Run research and supplement writers in parallel.
4. Respect every checkpoint; pause for user input.
5. Give the user enough context at each checkpoint to decide well.
6. No source code is generated; this pipeline produces documentation only.
7. The GDD is a living document: every draft carries its assumptions, open questions, and a decision log so it can be updated rather than rewritten.
