# What Makes a Great Game Design Document

This document records the research behind Game Doc Forge 2.0: the principles the templates and interviews are built on, the section tiers, the anti-patterns the auditor checks for, and the coverage checklists. The `gdd-writer` and `gdd-auditor` roles read it at run time; humans maintaining the plugin should read it before changing a template or a question module. Sources are listed at the end and cited inline by short name.

---

## 1. Principles

### 1.1 The document's job is alignment, not completeness

Nobody reads a hundred-page design bible. Stone Librande's GDC talk "One-Page Designs" (Librande) argues that whether it is a bible, a classic GDD, or a wiki, "nobody reads them," and that forcing each system onto one annotated page makes the designer "actually really understand the problem." The 2024 Game Developer guide (GD-2024) puts it the same way: "the hard part is getting everyone to agree on what the game is." Document360 names length as the first reason documentation goes unread.

**How the plugin applies it:** every GDD opens with a one-page vision layer that must stand alone (pitch, pillars, non-goals, experience goals, audience, USPs, scope, risks). Sections below it are modular and scannable: tables over prose, a summary line at the top of each section, no restating the heading.

### 1.2 Staged fidelity: concept, one-pager, full document

Tim Ryan's "Anatomy of a Design Document" (Ryan) separates the concept (one to two pages), the proposal (adds market, technical, legal, and cost analysis), and the functional and technical specs that come later. Rouse separates the focus, the design document, the story bible, and the technical design document. Game Dev Beginner's ladder is one page, ten pages, full. The Level Design Book (LDB) sizes the checklist to the team: Minimal (pitch, tools, scope), Recommended (adds pillars, experience goals, mechanics, asset list), Full (adds pacing, research, worldbuilding, weekly scope).

**How the plugin applies it:** `concept.md` is the concept stage; the interview's quick/standard/comprehensive depths map to the LDB ladder; the GDD template marks sections Core, Recommended, or Optional so a jam project and a commercial pitch use the same template at different fidelities. Supplements carry the spec-level detail so the GDD stays readable.

### 1.3 Living, modular, owned

Nuclino, Slite, GitBook, Drafft, and Whimsy agree: write in modular sections that update independently, cross-link, keep it searchable, and give one person ownership of the structure. Whimsy lists "No Clear Owner" as a named failure mode; Nuclino warns against Word files "locked away on someone's hard drive." The OpenGame GDD (2025) ships as standalone Markdown chapters "optimized for AI agent iteration, Git-based review workflows."

**How the plugin applies it:** Markdown output, a Document History table, a Decision Log appendix, an Open Questions appendix with owners, and `/gdd:resume` paths for revising a section without rewriting the document.

### 1.4 Pillars first; everything filters through them

Design pillars are "the 3-5 main elements or emotions a game aims to explore"; during production the test is "does this idea serve the pillars?" (GD-Pillars). Examples: The Last of Us = crafting, story, AI partners, stealth; Breath of the Wild = exploration, traversal, scavenging, options, combat. Bad pillars are slogans ("make a fun game", "the player must have fun") or tasks; good pillars are concrete enough to cut a feature over (ch0m5). GitBook's first section is "elevator pitch, then design pillars and non-goals."

**How the plugin applies it:** `concept-gatherer` will not accept a pillar the designer cannot name a cut for. Every system section in the GDD names the pillar it serves. The auditor flags features that fight a pillar without a stated tension.

### 1.5 Experience goals over feature lists

MDA (Hunicke, LeBlanc, Zubek) has designers build Mechanics to Dynamics to Aesthetics while players experience them in reverse; the eight aesthetics are Sensation, Fantasy, Narrative, Challenge, Fellowship, Discovery, Expression, Submission. Tynan Sylvester: games are "artificial systems for generating experiences"; when tuning a jump, ask "what emotion are you trying to provoke?" Fullerton's playcentric approach starts from "emotion-focused experience goals." Schell's lenses are all questions: Lens of Emotion, Essential Experience, Surprise, Curiosity, Problem Solving, Flow, Risk Mitigation, Skill vs Chance, and so on.

**How the plugin applies it:** the concept interview asks for the player fantasy, two or three target feelings and when they land, and the best imaginable moment before it asks about a single mechanic. The template's Player Experience section precedes Core Gameplay.

### 1.6 Specific, buildable, carrying its "why"

Ryan's failure list includes "insufficient specificity regarding intent," "contradictory or ambiguous language," "over-scoped feature lists," and "magic numbers unsupported by sources." GD-2024 warns against "copy this mechanic from Game X" with no explanation; Niederberger argues that understanding why a choice exists matters more than the spec. Ubisoft's Rational Game Design expresses difficulty as "atomic parameters" in a matrix so level docs are numeric, not vibes.

**How the plugin applies it:** the ledger records the reason next to each decision; the writer carries rationale into consequential sections (platform, business model, player count, resolution mechanic, difficulty model) and marks assumptions inline; comparables are always "keep X, change Y," never a bare list.

### 1.7 Keep pitch and design separate, but keep both

Whimsy names "Mixing Pitch and Design" as a mistake. The high-concept document is "a sales tool, like a resume for a video game" (McMaster). Publishers in 2025 want a playable build, comparables in the same budget band, and a path to profit (Chucklefish; gameloom).

**How the plugin applies it:** the vision layer doubles as the pitch source; the Market Positioning section is Optional and explicitly separate from design sections; the tabletop template carries a Sell Sheet section and lists it as a supplement.

### 1.8 Plan the expensive-to-retrofit systems early

Accessibility ("the way certain systems are designed can make or break accessibility features" - GAG), localization (30% text expansion, no hard-coded strings, pseudo-localization - Loc-10), telemetry ("start with the KPI, not the event menu" - GameAnalytics), save systems (save-anywhere multiplies test surface - Save-Design), platform certification (controller disconnect, suspend/resume, naming, privacy), and age ratings (the IARC questionnaire drives content decisions) are cheap on paper and ruinous to bolt on late. For tabletop, the equivalents are box and shipping class ("treat shipping as a design-stage constraint" - LaunchBoom), component count versus price point, colour-blind-safe iconography, and language-independent components.

**How the plugin applies it:** the video interview has dedicated modules for onboarding, accessibility and localization, technical foundations, and production; the tabletop interview has modules for components and production, rulebook and teach, and playtesting. All are Core or Conditional, never comprehensive-only.

---

## 2. Section tiers - video game GDD

Derived from the union of Chris Taylor's template (via LazyHatGuy's Markdown port), Ryan, Nuclino, GD-2024, OpenGame, the Unity/Oculus checklist, Slite, GitBook, Drafft, Kevuru, Observer, Game Dev Beginner, Into Games, Whimsy, Baldwin, Bethke, the McMaster high-concept template, LDB, and the Game Development Canvas. **Core** = in essentially every modern template and alignment-critical. **Recommended** = in most templates or strongly advised for a genre or business model. **Optional** = project-specific.

| Tier | Section | Level | Notes |
|------|---------|-------|-------|
| 0 Front matter | Title, logline, version, date, owner | Core | Unity "Logline"; Whimsy owner rule |
| 0 | Document history / decision log | Core | Taylor "Version History"; OpenGame traceability; GitBook changelogs |
| 0 | Confidentiality notice | Optional | Only when circulated externally |
| 1 Vision | Elevator pitch / high concept | Core | |
| 1 | Design pillars (3-5) | Core | |
| 1 | Non-goals ("what this game is not") | Recommended | GitBook; Ryan scope failures |
| 1 | Player fantasy and experience goals | Core | MDA, Sylvester, Fullerton |
| 1 | Genre, platform, input | Core | |
| 1 | Target audience | Core | Unity "Ideal User Profiles" |
| 1 | USPs and hooks | Core | |
| 1 | Comparables ("X meets Y") | Recommended | Same budget band; one hit, one average, one flop |
| 1 | Key features | Core | |
| 1 | Game flow summary | Recommended | Taylor; Bethke |
| 1 | Look and feel summary | Recommended | |
| 1 | Scope (counts) | Core | Taylor "Project Scope" |
| 1 | Business model and price | Core for commercial; Optional for jam or hobby | |
| 2 Gameplay | Core loop (second / minute / session / meta) | Core | |
| 2 | Player verbs | Core | Rouse "what can the player do?" |
| 2 | 3Cs: character, camera, controls; game feel targets | Core for real-time games | Swink; Ubisoft |
| 2 | Mechanics and systems | Core | |
| 2 | Objectives, win/lose, fail states | Core | |
| 2 | Progression and rewards | Core | |
| 2 | Difficulty and challenge curve | Recommended | RGD; flow |
| 2 | Game modes | Recommended | |
| 2 | Economy (sources, sinks, converters) | Recommended; Core for F2P or live | |
| 2 | Onboarding / FTUE | Recommended | Roblox: "get to the fun quickly" |
| 2 | Replayability, saving, options | Recommended | |
| 2 | Multiplayer / netcode | Core if multiplayer | |
| 2 | Social, community, UGC | Optional | |
| 2 | AI | Recommended | |
| 3 Content | Story, setting, tone | Recommended; Core for narrative games | |
| 3 | Characters | Recommended | |
| 3 | Narrative delivery mechanics | Recommended | Whimsy: define early to avoid UI rework |
| 3 | Levels / world structure, critical path, pacing | Recommended | LDB |
| 3 | Content volume table | Recommended | |
| 3 | Asset lists | Optional (production phase) | |
| 4 Presentation | UI/UX: screen flow, HUD, wireframes | Core | |
| 4 | Art direction | Core | |
| 4 | Audio and music direction | Recommended | |
| 4 | Accessibility plan | Recommended; Core for console or publisher titles | GAG Basic; XAG 101-120 |
| 4 | Localization plan | Recommended | Loc-10 |
| 5 Technical | Engine, target hardware, performance targets | Core | |
| 5 | Save / persistence | Recommended | |
| 5 | Telemetry plan | Recommended; Core for live or mobile | |
| 5 | Certification notes | Optional; Core for console | |
| 5 | Security / anti-cheat | Optional; Core for competitive online | |
| 5 | Tools | Optional | |
| 6 Production | Team and roles | Recommended | |
| 6 | Milestones (prototype, vertical slice, alpha, beta, cert, launch) | Recommended | Cerny "publishable first playable" |
| 6 | Budget | Recommended; Core for pitches | |
| 6 | Risks and mitigations | Core | Taylor; Ryan; Schell |
| 6 | Open questions | Recommended | |
| 6 | Success metrics | Recommended | |
| 6 | Market, marketing, post-launch | Optional; Core for commercial | |
| 6 | Ratings and legal | Optional | |
| 6 | Playtest plan | Recommended | |
| 6 | Glossary | Optional | |

**The minimum viable GDD** (the intersection of every template) is: pitch, pillars, experience goals, audience, platform, core loop and verbs, scope, art and UI direction, risks. That intersection is the one-pager.

---

## 3. Section tiers - tabletop GDD

Derived from the Ludology / Cardboard Edison checklist, BGDF design-document threads, Stonemaier's pitching, evaluation, playtesting, and MSRP posts, sell-sheet guides (Rock Manor, Big Potato, Hero Time, Meeple Mountain), rulebook guides (Hero Time, Boardssey), manufacturing guides (Gate Keeper, LaunchBoom, BGDF), the BGG weight thread, Engelstein and Shalev, and the Kobold Guide.

| Section | Level | Notes |
|---------|-------|-------|
| Sell-sheet vitals: title, player count, age, time, hook | Core | Meeple Mountain |
| Core experience and why someone plays it | Core | Ludology |
| Target audience and weight (BGG 1-5) | Core | Weight = rules count, playtime, luck, choices, bookkeeping |
| Differentiation vs market (familiarity + innovation) | Core | Big Potato; Stonemaier "close facsimile?" |
| Theme and player role (who am I, what do I want, how do I win) | Core | BGDF |
| Core mechanisms (named) and theme integration | Core | Engelstein taxonomy |
| Turn / round / phase flow and game arc | Core | |
| Decision points and tension | Core | Jface: constraints, planning, risk, reading, interaction, resolution, consequence, optionality, recovery |
| Player interaction model | Core | |
| End condition and scoring | Core | |
| Balance: catch-up, runaway leader, dominant strategies, start positions, elimination, downtime, luck vs skill | Core | Ludology; Games Precipice |
| Player-count scaling; solo/automa | Recommended | Mahtgician |
| Component manifest (counts, sizes, sheet math, box) | Core | |
| Cost of goods and MSRP | Recommended; Core if self-publishing | Fantastic Factories; Stonemaier |
| Rulebook outline (overview, components, setup, gameplay, end and scoring, reference) | Core from mid-development | Hero Time; Boardssey |
| Teach script (under 10 minutes) | Recommended | Streamlined Gaming |
| Graphic design and accessibility (fonts, icons, colour-blind, language-neutral) | Recommended | Ludology |
| Prototype log (version, date, question tested) | Core | Indiana template |
| Playtest log and plan (internal, targeted, blind x3, stress) | Core | Stonemaier "three waves of blind playtesting"; ready at ratings of 8-10 |
| Variants, expansions, modules | Optional | |
| Publisher targeting and pitch pack | Recommended | Stonemaier 4 steps |
| Designer bio | Optional | |
| Open questions and known problems | Recommended | |
| Production and market-entry strategy | Optional | |
| TTRPG additions: resolution system, character creation, GM tools, campaign structure | Core for TTRPGs | |

---

## 4. Anti-patterns

The auditor checks for these; the writer avoids them.

1. **The unread document**: too long, no visuals, no hierarchy (Librande; Document360; Slite's "explain it to a ten-year-old" test).
2. **Rouse's five**: the Wafer-Thin or Ellipsis Special ("gameplay will be fun..."); the Back-Story Tome (hundreds of pages of lore, no mechanics); the Overkill Document (frame counts and pseudocode in the design doc); the Pie-in-the-Sky Document (infeasible technology); the Fossilized Document (never updated, so the team stops trusting it).
3. **Ryan's failures**: insufficient gameplay description; unfun core mechanic; unrealistic resource demands; magic numbers unsupported by sources; inflexibility; failing to anticipate stakeholder concerns; condescending technical instructions to skilled professionals; drifting vision during writing.
4. **Pillar failures**: vague ("fun", "immersive"), a task masquerading as a pillar, more than five, pillars never used as a filter.
5. **Ownership failures**: no owner; the lead designer's private artefact; abandoned after kickoff.
6. **Prescriptive over descriptive**: specs with no goals "can stifle creativity" (Drafft).
7. **Excessive detail too early**: "attempting to map every feature on day one" (gamedesigning.org); lock the core loop first.
8. **Mixing pitch and design** (Whimsy).
9. **Reference by pointer**: "copy the mechanic from Game X" with no explanation (GD-2024).
10. **Bloat from fear of deleting** outdated material (Nuclino).
11. **UI, accessibility, and localization as afterthoughts** (Kevuru; Whimsy; Loc-10).
12. **AAA comparables in an indie pitch** signal weak market awareness (gameloom).
13. **Tabletop-specific**: designing the whole game before a 10-20 card MVP (Stonemaier); skipping blind playtests; rules that work as reference but not as tutorial; components that do not support the price point; ignoring box and shipping until late.

---

## 5. Coverage checklists

The auditor applies the checklist for the game family. Each item is **covered**, a **gap**, or **skipped** (the ledger has a `skipped` row with a reason). Items marked (C) are required at every depth; the rest may be gaps at `quick` depth but must be listed in Open Questions.

### 5.1 Video game

Vision
- (C) Pitch in three sentences or fewer
- (C) Three to five pillars, each with a stated cut
- (C) Non-goals listed
- (C) Player fantasy and at least two experience goals with the moment they land
- (C) Target player described; who it is not for
- (C) Comparables as "keep X, change Y"
- (C) USPs
- (C) Scope table with counts

Gameplay
- (C) Core loop at two or more timescales
- (C) Verb list with core versus secondary
- (C) Objectives, win, lose, and what failure costs
- Camera model, control scheme, remapping (real-time games)
- The one interaction that must feel great first
- Each system names the pillar it serves
- Resources with sources, sinks, converters
- Progression spine with pacing per hour
- Input versus output randomness identified
- Difficulty model (static or dynamic; options)
- First two minutes, first ten, first hour
- Replayability basis
- Save model (anywhere, checkpoint, permadeath) and what persists
- Multiplayer model: authority, session size, matchmaking (if multiplayer)
- Moderation and anti-cheat posture (if online with chat, UGC, or ranking)

Content
- Setting, tone, protagonist wants and obstacles (if narrative)
- Story delivery mechanics and mandatory versus optional story
- Level or area list with critical path and pacing
- Content volume table

Presentation
- (C) Art direction in one sentence plus references
- (C) Screen flow and HUD contents
- Audio direction; which sounds carry gameplay information and their visual redundancy
- Accessibility: remapping, text size, contrast, colour never the sole signal, subtitles, separate volume sliders
- Localization: launch languages, 30% expansion, externalized strings, glossary

Technical
- (C) Engine and rationale
- (C) Target platforms with performance targets
- Biggest technical unknowns and the prototype for each
- Telemetry: the top KPI and first event buckets (live or mobile)
- Certification constraints that affect design (console)

Production and business
- (C) Business model and price (commercial) or an explicit "not commercial"
- (C) Team, missing roles
- (C) Milestones including the smallest publishable version
- (C) Top risks with mitigations
- (C) Open questions with owners
- Success metrics
- Playtest plan
- Rating target and legal or IP exposures

### 5.2 Tabletop

Vitals and experience
- (C) Title or working title, player count and best count, age, time (first play and repeat), weight target
- (C) Core experience and why someone plays it
- (C) Target audience tier
- (C) Differentiation from the shelf neighbours
- (C) Theme, player role, individual and collective goal
- (C) Hook in one sentence

Mechanisms and structure
- (C) Named core mechanisms and their theme fit
- (C) Turn / round / phase flow
- (C) Resources and how they progress a player toward the goal
- (C) Decision points and the central tension
- (C) Interaction model
- (C) End condition, scoring, and what drives the game to a conclusion
- Game arc (opening, midgame, endgame feel)
- Where luck lives; input versus output randomness

Balance and scaling
- (C) Catch-up and runaway-leader handling
- Dominant strategy risks and counters
- Start-position and turn-order balance
- Elimination and downtime
- Per-player-count changes; solo or automa mode decision
- New-player-beats-veteran frequency target

Components and production
- (C) Component manifest with counts
- Card sheet math, punchboard, box, insert
- Price point and cost-of-goods sanity check
- Colour-blind-safe and language-neutral design
- Iconography and readability

Rules and teach
- (C) Rulebook outline
- Teach script under ten minutes
- Reference aids
- Known ambiguities

Validation
- (C) Prototype log with the question each version tests
- (C) Playtest plan with stages and stop criterion
- Session log fields

Publishing
- Route: self-publish, crowdfund, or pitch
- Sell sheet contents
- Target publishers or shelf
- Variants and expansions parked

TTRPG additions
- (C) Core resolution mechanic
- (C) Character creation
- GM tools and prep burden
- Campaign or one-shot structure

---

## 6. Sources

Philosophy and modern GDDs
- Librande - One-Page Designs: https://www.gamedeveloper.com/design/video-one-page-designs ; https://gdcvault.com/play/1012356/One-Page
- Ryan - Anatomy of a Design Document, parts 1 and 2: https://www.gamedeveloper.com/design/the-anatomy-of-a-design-document-part-1-documentation-guidelines-for-the-game-concept-and-proposal ; https://www.gamedeveloper.com/design/the-anatomy-of-a-design-document-part-2-documentation-guidelines-for-the-functional-and-technical-specifications
- Rouse - Not All Game Design Documents Are Created Equal: https://www.gamedeveloper.com/design/game-design-theory-practice-second-edition-not-all-game-design-documents-are-created-equal-
- GD-2024 - How to write a game design document: https://www.gamedeveloper.com/design/how-to-write-a-game-design-document
- Nuclino: https://www.nuclino.com/articles/write-game-design-document
- Slite: https://slite.com/learn/game-design-document
- Document360: https://document360.com/blog/write-game-design-document/
- GitBook: https://www.gitbook.com/blog/how-to-write-a-game-design-document
- Drafft: https://drafft.dev/blog/game-design-document-guide
- Whimsy: https://whimsygames.co/blog/game-design-instructions-examples/
- gamedesigning.org: https://gamedesigning.org/learn/game-design-document/
- Game Dev Beginner: https://gamedevbeginner.com/how-to-write-a-game-design-document-with-examples/
- Into Games: https://intogames.org/news/how-to-write-game-design-document
- Kevuru: https://kevurugames.com/blog/how-to-write-a-game-design-document-gdd/
- Observer Games (2025): https://www.observer.games/2025/10/24/the-ultimate-game-design-document-template-a-blueprint-for-building-better-games/
- GD-Pillars: https://www.gamedeveloper.com/design/design-pillars-the-core-of-your-game ; ch0m5: https://ch0m5.github.io/Game-Design-Pillars/
- MDA: https://en.wikipedia.org/wiki/MDA_framework
- Sylvester - Designing Games (notes): http://paulgestwicki.blogspot.com/2025/08/notes-from-tynan-sylvesters-designing.html
- Fullerton - Game Design Workshop: https://www.routledge.com/Game-Design-Workshop-A-Playcentric-Approach-to-Creating-Innovative-Games/Fullerton/p/book/9781032607009
- Schell - The Art of Game Design, Deck of Lenses: https://www.amazon.com/Art-Game-Design-Deck-Lenses/dp/B0FVB9QQCJ
- Game Development Canvas: https://www.gamedeveloper.com/production/game-development-canvas---a-new-method-of-presenting-and-managing-a-game-project
- Cerny Method: https://www.slideshare.net/holtt/cerny-method

Templates with section lists
- LazyHatGuy (Chris Taylor lineage): https://github.com/LazyHatGuy/GDDMarkdownTemplate
- saeidzebardast: https://github.com/saeidzebardast/game-design-document
- OpenGame GDD: https://opengame.borninsea.com/game-design-document-template/
- Unity/Oculus GDD Checklist: https://connect-prd-cdn.unity.com/20191004/4be5cf7a-c2b2-48f8-bbdb-6c0233df7a0e/Game%20Design%20Document%20Checklist.pdf
- Chris Taylor template: https://www.gamedev.net/tutorials/game-design/game-design-and-theory/chris-taylors-design-document-template-r1063/
- Baldwin template: https://www.scribd.com/document/37138054/Baldwin-Game-Design-Document-Template
- McMaster high-concept document: https://www.cas.mcmaster.ca/~carette/SE4GP6/2017_18/highconcept.html
- LDB - Level Design Book: https://book.leveldesignbook.com/process/preproduction ; https://book.leveldesignbook.com/process/layout/criticalpath
- Mullich - Actionable GDD template: https://davidmullich.com/2018/06/25/an-actionable-game-design-document-template/
- Indiana template: https://sites.google.com/view/indiana-game-design-template/prototypes-and-playtesting

Coverage areas
- GAG - Game Accessibility Guidelines: https://gameaccessibilityguidelines.com/full-list/
- XAG - Xbox Accessibility Guidelines: https://learn.microsoft.com/en-us/gaming/accessibility/guidelines
- Loc-10 - localization rules: https://www.gamedeveloper.com/business/how-to-prepare-a-game-for-localization-10-basic-rules ; https://www.gridly.com/blog/game-ui-design-localization-best-practices/
- GameAnalytics - events to track first: https://www.gameanalytics.com/blog/what-events-should-you-track-first-game-analytics ; KPIs: https://appfollow.io/blog/mobile-game-kpis
- Economy and live-ops excerpt: https://www.gamedeveloper.com/design/book-excerpt-game-economy-design-metagame-monetization-and-live-operations
- FTUE: https://create.roblox.com/docs/production/game-design/onboarding ; https://nastyrodent.com/onboarding-and-ftue-design/
- 3Cs: https://www.gamedevpills.com/p/the-3cs-framework-character-camera
- Game feel (Swink): https://lizengland.com/blog/review-game-feel-by-steve-swink/
- RGD - Rational Game Design: https://dev.to/goldenxp/rational-game-design-in-a-hurry-1m08 ; https://www.gamedeveloper.com/design/rational-design-the-core-of-i-rayman-origins-i-
- Flow and difficulty: https://www.bloodmooninteractive.com/articles/flow-theory.html
- Save-Design: https://www.gamedeveloper.com/design/save-system-design-pt-3
- Certification: https://room8group.com/solutions/qa/game-compliance-and-certification-testing/
- Ratings: https://www.esrb.org/ratings-guide/ ; https://www.skala.io/blog/age-ratings-for-games-a-practical-guide-for-game-development-startups
- Netcode: https://www.gamedeveloper.com/game-platforms/online-multiplayer-the-hard-way ; https://create.roblox.com/docs/projects/server-authority
- Moderation: https://www.conectys.com/blog/posts/what-is-in-game-moderation-the-ultimate-guide-for-gaming-companies/
- Pitching: https://chucklefish.org/blog/guide-for-pitching-to-publishers/ ; https://gameloom.ai/blog/how-to-pitch-a-game-to-publishers ; https://indiegamebusiness.com/state-of-pitching-in-2025/

Tabletop
- Ludology / Cardboard Edison checklist: https://static1.squarespace.com/static/55fc10b1e4b0347ac88a7992/t/570149b01bbee0d8252e5877/1459702192564/Ludology+Game+Design+Checklist.pdf
- BGDF threads: https://www.bgdf.com/forum/game-creation/design-theory/design-documents-used-or-not-used ; https://www.bgdf.com/forum/game-creation/playtesting/what-kinds-questions-do-you-ask-playtesters
- Stonemaier: https://stonemaiergames.com/4-steps-to-pitch-your-game-to-a-tabletop-publisher/ ; https://stonemaiergames.com/how-do-we-decide-which-games-to-publish/ ; https://stonemaiergames.com/tabletop-game-prototyping-playtesting-and-development/ ; https://stonemaiergames.com/kickstarter-lesson-59-the-myth-of-msrp/
- Fantastic Factories on MSRP: https://fantastic-factories.medium.com/setting-the-msrp-of-your-board-game-and-why-you-shouldnt-use-the-5x-landed-cost-rule-685808eb7e5f
- Meeple Mountain: https://www.meeplemountain.com/top-six/top-six-tips-for-pitching-your-game-to-publishers/
- Sell sheets: https://rockmanorgames.com/2016/06/28/how-to-make-a-professional-looking-sell-sheet/ ; https://bigpotato.com/blogs/blog/how-to-make-a-board-game-sell-sheet-and-pitch-video ; https://herotime1.com/academy/marketing/board-game-sell-sheets-complete-guide/
- Rulebooks: https://herotime1.com/academy/design/how-to-write-a-rulebook-for-your-board-game/ ; https://boardssey.com/blog/writing-your-board-game-rulebook-a-complete-guide
- Playtesting: https://boardssey.com/blog/board-game-playtesting-why-most-designers-fail ; https://entrogames.substack.com/p/what-to-do-after-the-playtest-20-playtesting-questions-that-set-players-up-to-give-great-answers ; https://www.backerkit.com/blog/playtest-feedback-form/
- Board Game Design Lab: https://boardgamedesignlab.com/how-to-design-a-board-game/
- Streamlined Gaming: https://streamlinedgaming.com/10-questions-to-ask-before-finalizing-your-board-game-design/
- Jface checklist: https://jfacegames.substack.com/p/board-game-design-checklist
- Theme and mechanics: https://www.gamedeveloper.com/blogs/how-to-marry-the-theme-and-mechanics-in-your-board-game
- Player count: https://mahtgiciangames.com/blogs/the-creative-workshop-game-design-blueprints/designing-for-different-player-counts-solo-to-multiplayer ; https://www.gamesprecipice.com/player-count-scalability/
- BGG weight: https://boardgamegeek.com/thread/3007318/weight-complexity-rating-guidance
- Manufacturing: https://www.gatekeepergaming.com/article-11-demystifying-game-components/ ; https://www.launchboom.com/game-tips/board-game-manufacturers/
- Engelstein and Shalev - Building Blocks of Tabletop Game Design: https://www.routledge.com/Building-Blocks-of-Tabletop-Game-Design-An-Encyclopedia-of-Mechanisms/Engelstein-Shalev/p/book/9781032015811
- Kobold Guide to Board Game Design: https://koboldpress.com/kobold-guide-to-board-game-design/
- Cardboard Edison 2025 best practices: https://news.thegamecrafter.com/post/772307950624768000/cardboard-edison

Research compiled August 2026. Chris Taylor's original document and Schell's full lens table were not directly accessible; both are covered through faithful secondary sources.
