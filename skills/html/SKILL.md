---
name: html
description: Generate a sleek, self-contained HTML reading edition of a GDD session - a hub landing page, a game-flow page rendering the core loop and economy as diagrams, the GDD itself, and every supplement. Use when the user wants the design documents as web pages, wants to read or share the GDD in a browser, or asks for an HTML or visual version of a game design document.
---

# gdd:html - HTML reading edition

Convert a completed (or partially completed) GDD session into a set of self-contained HTML pages. The Markdown files stay the record; the HTML is the reading surface.

Treat the text supplied with the invocation as the project name. If none was supplied, list every directory under `.gdd/sessions/` with its `currentStage` and ask which one. If `.gdd/sessions/` does not exist, say so and suggest `/gdd:create`.

## Portable role loading

Resolve bundled files relative to this `SKILL.md`, not the working directory:

- role prompts: `../../agents/<role>.md`
- the HTML shell: `../../templates/html_shell.html`

Compute the absolute plugin root once and reuse it. Read a role's Markdown file before dispatching it. Do not depend on `gdd:<role>` agent-type registration; Codex discovers skills but does not register `agents/`.

## Preconditions

Read `.gdd/sessions/<project>/state.json`.

- No `GDD.md` on disk: stop and say the session has not reached GDD writing yet, and that `/gdd:resume <project>` will continue it. There is nothing worth rendering before that point.
- `GDD.md` present but the session is mid-pipeline: proceed, and tell the user which documents do not exist yet so the set's gaps are expected rather than surprising.

## Output

```
.gdd/sessions/<project>/html/
  index.html          hub: pitch, pillars, project state, document cards, reading paths, open questions
  game-flow.html      core loop, timescales, match structure, economy sources and sinks, progression
  gdd.html            the full GDD
  <supplement>.html   one per supplement, same slug as the Markdown file
```

Every page is self-contained: no CDN, no webfont, no network asset. The pages open from `file://` with no server and no connection.

## Pipeline

### 1. Inventory

List what exists: `GDD.md`, every file in `supplements/`, and (only if the user asks for them) `research/`, `research_synthesis.md`, `interview-ledger.md`, `INDEX.md`. Read `INDEX.md` if present - its descriptions and reading paths feed the hub directly and are better than anything invented fresh.

Build the **page manifest**: for each page, its output filename, title, kicker (`Core document` / `Supplement` / `Reference`), a one-line description, and approximate size. Order by reading priority: game flow, GDD, then supplements in the order `supplement-plan.md` recommends, then reference material.

### 2. Build the shared nav

Generate the `{{NAV}}` fragment once, from the manifest, and pass the **same string** to every agent:

```html
<a href="index.html">Overview</a>
<a href="game-flow.html">Game flow</a>
<a href="gdd.html">Game Design Document</a>
<a href="tech-tree-reference.html">Tech Tree Reference</a>
```

Each agent adds `aria-current="page"` to its own link. Generating nav per-agent produces divergent sidebars and is the most common way this stage goes wrong.

### 3. Dispatch, in parallel

One `html-designer` per document (GDD and each supplement), plus one `html-hub-designer` for `index.html` and `game-flow.html`. Send them in a single batch - they are independent and each owns exactly one output file, so there is no write conflict.

Give every agent: the shell path, the shared nav, the project name, its source path, its exact output path, its kicker and subtitle, and the full body of its role file.

Dispatch the hub designer in the same batch. It reads `GDD.md` and `concept.md` directly rather than waiting on the other pages, and it already has the manifest.

### 4. Verify

After the batch returns, check on disk - do not take the agents' word for it:

1. Every manifest page exists and is non-empty.
2. No file contains a literal `{{` - an unreplaced placeholder is a visible defect.
3. No page references a network asset. Grep for `src="http`, `href="http` inside `<link>`, and `@import`. Links to real external sources inside prose are fine and expected.
4. Every `href="*.html"` in the nav resolves to a file that exists.

Fix anything trivial yourself. Re-dispatch only the single agent whose page failed.

### 5. Report

Print the output directory, the page list with sizes, and how to open it. Surface anything an agent flagged - the hub designer in particular is asked to report design holes its diagrams exposed, such as a resource with no sink or a loop that does not close. Those findings come only from this stage and are worth raising.

## State

If `state.json` exists, record the stage without disturbing the pipeline's own progression:

- add `"htmlGeneration"` to `stageOutputs` as the list of generated files
- add `"HTML_GENERATION"` to `completedStages` if not present
- update `updatedAt`

Do **not** change `currentStage` when the session is already `COMPLETED`; this skill is re-runnable and must not appear to reopen a finished pipeline.

## Re-running

Re-running regenerates every page. Users edit the Markdown and re-run to refresh, so never merge into an existing HTML file - overwrite it. Warn before overwriting only if a page's modification time is newer than its source Markdown, which suggests someone hand-edited the HTML.

## Key behaviors

1. The Markdown is authoritative. HTML that disagrees with its source is a bug in the HTML.
2. Never drop content for length. These documents are long; the pages are long.
3. Never resolve an open question or promote an assumption while converting.
4. One agent, one output file.
5. Self-contained always - these pages get emailed, put on USB sticks, and opened on planes.
