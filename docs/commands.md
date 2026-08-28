# GDD Pipeline Commands Reference

## Overview

These commands orchestrate the Game Doc Forge pipeline for creating Game Design Documents. Claude Code invokes them as `/gdd:<command>`; Codex as `$gdd:<command>`.

---

## Command: `/gdd:create`

**Usage:**
```
/gdd:create [project_name]
```

**Arguments:**
- `project_name` (optional): Name for this GDD project. If not provided, you will be prompted.

**Process:**
1. **Concept Gathering** (inline): game family and type, pitch, comparables, hook, player fantasy and experience goals, design pillars with their cuts, audience, exclusions, purpose, team, budget and timeline, hard constraints, success measure, known unknowns
2. **Interview** (inline, forked by family): depth checkpoint, then the video-game or tabletop module set
3. **Research Decision Checkpoint**
4. **Research Planning** (if enabled): topics prioritized by which open ledger items they can close
5. **Research Approval Checkpoint**
6. **Research Execution** (parallel)
7. **Research Vetting**: synthesis plus a table of ledger items the research resolved
8. **GDD Writing**: family template, traceability rules
9. **GDD Audit**: traceability, completeness, consistency, anti-patterns; blocking findings send the writer back once
10. **GDD Review Checkpoint**
11. **Supplement Analysis**
12. **Supplement Selection Checkpoint**
13. **Supplement Generation** (parallel)
14. **Index Generation**
15. **HTML Decision Checkpoint**
16. **HTML Generation** (parallel): hub, game-flow diagrams, GDD, and every supplement as self-contained pages

**Output:**
```
.gdd/sessions/<project_name>/
  state.json
  concept.md
  interview-ledger.md
  details.md
  research-plan.md          (optional)
  research/                 (optional)
  research_synthesis.md     (optional)
  GDD.md
  gdd-audit.md
  supplement-plan.md
  supplements/
  INDEX.md
```

**Example:**
```
/gdd:create stellar-conquest
/gdd:create "My Card Game"
/gdd:create
```

---

## Command: `/gdd:resume`

**Usage:**
```
/gdd:resume [project_name]
```

**Process:**
1. Read `state.json`; migrate 1.0 sessions (`DEEP_DIVE` becomes `INTERVIEW`, new keys added)
2. Display the progress summary including interview coverage
3. Report partial work (unfinished interview modules, missing research or supplement files)
4. Offer: continue, restart the current stage, or go back to the previous checkpoint
5. Continue the pipeline, reusing every existing output

For completed sessions, offer instead: resolve open questions, revise the GDD, extend the interview with skipped modules, generate more supplements, or rebuild the index.

---

## Command: `/gdd:status`

**Usage:**
```
/gdd:status [project_name]
```

**Output Example:**
```
Pipeline Status: stellar-conquest
----------------------
Game: video / pc-console - 4X strategy

[x] Concept Gathering     - completed
[x] Interview             - completed (video-game, standard depth, 12/12 modules)
[-] Research              - skipped (user chose no research)
[x] GDD Writing           - completed
[ ] GDD Audit             - in_progress
[ ] GDD Review            - pending
[ ] Supplement Analysis   - pending
[ ] Supplement Selection  - pending
[ ] Supplement Generation - pending
[ ] Index Generation      - pending

Interview ledger: 52 decided, 7 assumed, 5 open, 3 skipped
Current Stage: GDD_AUDIT
Last Updated: 2026-08-24T18:30:00Z
Checkpoint decisions: depth=standard, research=skip, gddReview=-, supplements=-
```

---

## Checkpoint Interactions

### Interview Depth Checkpoint

At the start of the game-type interview:

```
How deep should this interview go?

1. Quick        - 20-30 questions, ~15 min. Core modules only; everything else
                  becomes an explicit assumption or open question.
2. Standard     - 45-70 questions, ~35 min. All applicable modules; secondary
                  probes where answers are thin.
3. Comprehensive - 90+ questions, 60+ min. Every module, every probe, plus
                  genre-specific probes. Aims to leave no GDD section on an assumption.

You can change depth at any time ("go deeper on combat", "speed this up").
```

During the interview every question also accepts: **you decide** (recorded as an assumption with rationale), **skip** (not applicable), and **later** (recorded as an open question).

### Research Decision Checkpoint

```
Your concept and interview are captured.
Open questions research could help resolve: VG-02-05 (rating descriptors),
VG-13-02 (Switch performance budget), VG-14-04 (comparable wishlist numbers).

Would you like to conduct research before writing the GDD?

1. Conduct research (recommended for real-world settings, specific platforms,
   crowded markets, or when open questions are factual)
2. Skip research (proceed directly to GDD writing)
```

### Research Approval Checkpoint

```
Research Topics Identified:
---------------------------
Historical/Domain Research:
  1. [HIGH] Medieval siege warfare tactics                (resolves: VG-08-06)
  2. [MEDIUM] Castle architecture and defense

Market Research:
  1. [HIGH] Tower defense games 2025-2026                 (resolves: VG-14-04)
  2. [MEDIUM] Mobile strategy game monetization

Technical Research:
  1. [HIGH] Switch performance budgets for 2D strategy    (resolves: VG-13-02)
  2. [LOW] Cross-platform save systems

Select topics to research:
- Enter numbers (e.g., "1,2,5")
- Enter "all" for all topics
- Enter "high" for high priority only
- Enter "skip" to skip research
```

### GDD Review Checkpoint

```
GDD has been generated: .gdd/sessions/<project>/GDD.md

Document Overview:
- 14 sections, ~9,400 words
- Pillars: Every death teaches; Ten minutes to learn; The map is the story
- Consequential decisions: premium at USD 19.99; server-authoritative co-op for 2;
  checkpoint saves; three difficulty presets plus assists
- 7 assumptions, 5 open questions carried into the appendices
- Audit: 0 blocking, 2 major, 4 minor findings remaining (listed below)

Options:
1. Approve and continue to supplement analysis
2. Request revisions (specify sections)
3. Answer open questions now
4. Pause here
```

### Supplement Selection Checkpoint

```
Recommended Supplements:
------------------------
[HIGH PRIORITY]
  1. Unit Stats Table - Essential for balance implementation
  2. Playtest Plan - Five open items are playtest-dependent
  3. Vertical Slice Definition - Milestone exit criterion needs scope

[MEDIUM PRIORITY]
  4. Resource Balance Sheet - Economy tuning
  5. Accessibility Checklist - Console target

[LOW PRIORITY]
  6. Lore Bible - World building depth

Select supplements to generate:
- Enter numbers (e.g., "1,2,3")
- Enter "recommended" for HIGH priority
- Enter "all" for all supplements
- Enter "none" to complete without supplements
```

---

## State Management

Pipeline state is tracked in `.gdd/sessions/<project>/state.json` (see `templates/state_template.json` for the full schema).

```json
{
  "version": "2.0",
  "projectName": "project-name",
  "currentStage": "STAGE_NAME",
  "completedStages": ["CONCEPT_GATHERING", "INTERVIEW"],
  "gameInfo": { "family": "video", "type": "pc-console", "genre": "...", "pillars": ["..."] },
  "interview": {
    "role": "interview-video-game",
    "depth": "standard",
    "modulesPlanned": ["01", "02", "03", "05", "06", "07", "09", "10", "12", "13", "14", "99"],
    "modulesCompleted": ["01", "02", "03"],
    "counts": { "decided": 18, "assumed": 2, "open": 1, "skipped": 0 }
  },
  "checkpoints": {
    "interviewDepth": { "reached": true, "depth": "standard" },
    "researchDecision": { "reached": false, "decision": null }
  }
}
```

### Stage Names

- `NOT_STARTED`
- `CONCEPT_GATHERING`
- `INTERVIEW`
- `RESEARCH_DECISION`
- `RESEARCH_PLANNING`
- `RESEARCH_APPROVAL`
- `RESEARCH_EXECUTION`
- `RESEARCH_VETTING`
- `GDD_WRITING`
- `GDD_AUDIT`
- `GDD_REVIEW`
- `SUPPLEMENT_ANALYSIS`
- `SUPPLEMENT_SELECTION`
- `SUPPLEMENT_GENERATION`
- `INDEX_GENERATION`
- `COMPLETED`

`DEEP_DIVE` (1.0) is migrated to `INTERVIEW` by `/gdd:resume`.

---

## The Interview Ledger

`interview-ledger.md` records every question asked across the concept and interview stages:

| Column | Meaning |
|--------|---------|
| ID | `CG-mm-nn`, `VG-mm-nn`, or `TT-mm-nn` (prefix, module, question); stable, never renumbered |
| Module | Module number and name |
| Question | As asked |
| Answer | The decision in the designer's words |
| Status | `decided`, `assumed`, `open`, `skipped`, `superseded` |
| Notes | Rationale for assumptions, partial thinking for open items, supersession pointers |

The GDD writer may not state anything normative without a `decided` or `assumed` row; the auditor enforces this and checks the reverse (every decided row appears in the GDD; every assumed and open row appears in the appendices).

---

## Error Handling

If the pipeline encounters an error:

1. State is saved to allow resume
2. Error is logged to `state.json`
3. User is notified with options: retry the failed stage, skip to the next stage (if safe), or abort and save progress

An interrupted interview is not an error; the ledger holds every completed module and `/gdd:resume` continues at the next one.

---

## Output Files

| File | Contents |
|------|----------|
| `concept.md` | Pitch, key facts, hook, player experience, pillars, audience, exclusions, known unknowns, constraints |
| `interview-ledger.md` | Every question with answer and status |
| `details.md` | Synthesized design details by module, plus Assumptions and Open Questions |
| `research-plan.md` | Research topics with priorities and the ledger items each resolves |
| `research/*.md` | Historical, market, technical findings |
| `research_synthesis.md` | Consolidated research with confidence levels and ledger items addressed |
| `GDD.md` | The Game Design Document (video or tabletop template) |
| `gdd-audit.md` | Audit findings, coverage checklist, reverse traceability gaps |
| `supplement-plan.md` | Recommended supplements with priorities |
| `supplements/*.md` | Generated supplements |
| `INDEX.md` | Catalog of every file with descriptions and reading order |

---

## Command: `/gdd:html`

**Usage:**
```
/gdd:html [project_name]
```

**Arguments:**
- `project_name` (optional): the session to render. If omitted, lists the available sessions and asks.

**What it does:**

Converts a session's Markdown into a browser reading edition. The Markdown stays the record; the HTML is the reading surface, and the two say the same thing.

1. Inventories the session and builds a page manifest ordered by reading priority
2. Generates one shared sidebar navigation so every page agrees
3. Dispatches, in one parallel batch: one `html-designer` per document, plus one `html-hub-designer` for the hub and the game-flow page
4. Verifies on disk that no placeholder survived, no page reaches the network, and every nav link resolves

**Output:**
```
.gdd/sessions/<project_name>/html/
  index.html          hub: pitch, pillars, project state, document cards, reading paths, open questions
  game-flow.html      core loop, timescales, match structure, economy sources and sinks, progression
  gdd.html            the full Game Design Document
  <supplement>.html   one per supplement
```

**Properties:**
- **Self-contained.** No CDN, no webfont, no network asset. Pages open from `file://` with no server and no connection - they survive being emailed, put on a USB stick, or opened on a plane.
- **Theme-aware.** Light and dark, following the OS, with a toggle that overrides in either direction.
- **Responsive and printable.** The sidebar collapses on narrow screens; print drops the chrome.
- **Re-runnable.** Regenerates every page. Edit the Markdown, run it again.

**Requires:** `GDD.md` must exist. Before that there is nothing worth rendering; the skill says so and points at `/gdd:resume`.

**Note:** the `game-flow.html` page is the only page that is a synthesis rather than a conversion. It gathers the core loop, match structure, economy, and progression - which live as prose scattered across several GDD sections - and renders them as diagrams. It is also the only stage that looks at the design that way, so it occasionally surfaces a hole the prose hid, such as a resource with no sink or a loop that does not close.
