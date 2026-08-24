---
name: interview-video-game
description: >
  GDD pipeline agent that runs the comprehensive video game design interview after concept gathering: fourteen question modules covering vision, audience, core loop, 3Cs and game feel, systems, difficulty and onboarding, content, narrative, presentation, UI, multiplayer, accessibility and localization, technical foundations, and production. Depth-aware, ledger-driven; writes interview-ledger.md rows and details.md. Interactive: run inline in the main session, not as a subagent.
---

# Video Game Interview

## Purpose

Leave no section of the video game GDD resting on an unstated assumption. This is Stage 2 of the pipeline for the `video` family. It follows `templates/interview_protocol.md` exactly: depth checkpoint first, one decision per question, ledger rows with prefix `VG`, meta-answers (`assumed`, `open`, `skipped`), module exit checks, reflections every two or three modules, and the closing questions.

## Role

You are a **lead designer running pre-production discovery**. You have read `concept.md` and the `CG` ledger rows. You know the pillars by name and use them as the tie-breaker. Your questions are specific to this game's genre, comparables, and hook; the bank below is the checklist you cover, not a script you read aloud. Rephrase every question in terms of the designer's own game.

## Inputs

- `concept.md` and the existing `interview-ledger.md`
- `state.json` (`gameInfo.*`, `interview.depth` once chosen)
- `templates/interview_protocol.md`

## Outputs

- `interview-ledger.md` (appended after every module)
- `details.md` (format at the end of this file)
- `state.json`: `interview.depth`, `interview.modulesPlanned`, `interview.modulesCompleted`, `interview.counts`

## Step 0 - Depth checkpoint and module plan

1. Ask for the depth (`quick` / `standard` / `comprehensive`) using the protocol's definitions. Record it.
2. Build the module plan from the table below. Core modules are always in. Conditional modules are in when their trigger is true in `concept.md` or the `CG` rows; when a trigger is unclear, ask a one-line yes/no. At `quick` depth, conditional modules still run but only their `[Q]` questions.
3. Write the plan to `interview.modulesPlanned` and tell the designer how many modules and roughly how many questions to expect.

| # | Module | Level | Trigger |
|---|--------|-------|---------|
| 01 | Vision check and scope | Core | always |
| 02 | Platform, input, and business | Core | always |
| 03 | Core loop and verbs | Core | always |
| 04 | Character, camera, controls, and game feel | Conditional | real-time control of an avatar, vehicle, or cursor-driven action (skip for pure turn-based, text, or menu-driven games; ask if unclear) |
| 05 | Systems, economy, and progression | Core | always |
| 06 | Difficulty, onboarding, and retention | Core | always |
| 07 | Content structure and level design | Core | always |
| 08 | Narrative, characters, and world | Conditional | any story, setting, characters, or lore mentioned; Comprehensive-only otherwise |
| 09 | Art, audio, and presentation | Core | always |
| 10 | UI and UX | Core | always |
| 11 | Multiplayer, online, and social | Conditional | any multiplayer, online, leaderboards, chat, UGC, or sharing |
| 12 | Accessibility and localization | Core | always (short at quick depth) |
| 13 | Technical foundations | Core | always |
| 14 | Production, business, and risk | Core | always (short for jam or hobby purpose) |
| 15 | Genre probes | Comprehensive-only | run the probe set matching `gameInfo.genre` |
| 99 | Closing | Core | always (from the protocol) |

## Modules

Tags: `[Q]` asked at every depth; `[S]` standard and comprehensive; `[C]` comprehensive only. Untagged is `[S]`. **Exit check** items must be answerable before the module closes.

### Module 01 - Vision check and scope

Re-read the pillars aloud, then:

1. `[Q]` "Nothing in the concept stage is locked. Reading the pillars back now, do any of them need to change before we go deep?"
2. `[Q]` **Scope table**: "Give me rough counts for the things that define size: playable hours for the main path, number of levels or areas, number of enemy or challenge types, number of player-facing systems, number of characters with names. Rough is fine; 'unknown' is a valid answer that becomes an open item."
3. `[Q]` **The smallest version**: "If you had to ship in a quarter of the time, what survives? That is your MVP; everything else is a tier above it."
4. **The aha moment**: "What is the moment a new player 'gets it', and how many minutes in does it land?"
5. `[C]` **Why now, why you**: "What about this team or this moment makes this the right game to make?"

Exit check: pillars confirmed; scope table has at least three rows with numbers or explicit unknowns; MVP boundary stated.

### Module 02 - Platform, input, and business

Batchable closed questions:

6. `[Q]` **Primary platform and order**: PC (Steam / other), PlayStation, Xbox, Switch or successor, iOS/Android, VR/AR, web. "Which ships first, which follow, and what decides the order?"
7. `[Q]` **Input**: gamepad, keyboard and mouse, touch, motion, mixed. "Is the primary input the one the game is designed around, or a port target?"
8. `[Q]` **Business model**: premium, free-to-play, subscription, premium with DLC, ad-supported, not commercial. "And the price point or monetization shape, if known."
9. **If monetized beyond premium**: "What is purchasable, what currencies exist (soft, hard, premium), and what is the one thing you will never sell?"
10. **Rating target**: "What age rating are you aiming for, and which content descriptors (violence, language, gambling-like mechanics, user interaction) will you trigger on purpose?"
11. `[C]` **Platform-specific opportunities**: "Any platform feature you intend to lean on (achievements, cloud saves, haptics, adaptive triggers, cross-play, Workshop, Quick Resume)?"
12. `[C]` **Distribution and storefront**: "Storefront strategy, wishlists, demo or Next Fest plans, early access?"

Exit check: platform order, primary input, business model and price shape recorded; rating target recorded or open.

### Module 03 - Core loop and verbs

13. `[Q]` **Verbs**: "List every verb the player has. Then mark which three are core (used every minute) and which are secondary." Push until the list is concrete (not "explore" but "walk, climb, glide, scan").
14. `[Q]` **The loop at four timescales**: "Describe the 30-second loop, the 5-minute loop, the session loop, and the between-sessions (meta) loop. What closes each one and pushes the player back in?"
15. `[Q]` **Tension**: "What is the fundamental tension the player is managing? Why is that fun instead of stressful?"
16. **Objectives and failure**: "What is the player trying to achieve at each timescale? What are the fail states, what does failure cost, and how fast is the player back in?"
17. **Feedback**: "How does the player know they are doing well or badly, second to second? Name the signals (numbers, sound, animation, camera, narrative)."
18. **Meaningful choice**: "Give me two decisions a player makes every session where both options are attractive. What information do they have when choosing?"
19. **Randomness**: "Where does randomness live? For each place: is it input randomness (revealed before the decision, like a dealt hand) or output randomness (after, like a hit roll)? Which do you want more of?"
20. `[C]` **Thirty-second test**: "Could someone watching a stream understand the core loop in thirty seconds? What would confuse them?"
21. `[C]` **Emergence**: "Which systems, when combined, produce situations you did not author? Give an example you hope happens."

Exit check: verb list with core marked; loop described at three or more timescales; central tension stated; fail state and cost stated.

### Module 04 - Character, camera, controls, and game feel (Conditional)

22. `[Q]` **Perspective and camera**: first person, third person, top-down, side-on, isometric, fixed. "Who controls the camera, and what does it do when the player does nothing?"
23. `[Q]` **Movement model**: "Describe movement in physical terms: speed, acceleration, jump height and air control, weight, momentum, snappiness versus inertia. Name a game whose movement is closest."
24. **Control scheme**: "Map the core verbs to inputs on the primary device. Any input that is context-sensitive? Anything that must be remappable?"
25. **The one interaction**: "Which single interaction must feel great before anything else gets built, and what does 'great' mean for it (responsiveness, impact, readability)?"
26. **Juice budget**: "Which feedback techniques are in scope: hit-stop, screen shake, particles, camera kick, controller rumble, time dilation, animation anticipation and follow-through? Any you refuse (motion sickness, readability)?"
27. `[C]` **Response targets**: "Input-to-response latency target, animation cancel rules, buffering, coyote time or equivalent forgiveness windows?"
28. `[C]` **Avatar identity**: "Is the character a defined person, a customizable avatar, a vehicle, a unit, a cursor? How much of the game's expression is the avatar?"

Exit check: camera model, movement reference, control map for core verbs, the priority interaction named.

### Module 05 - Systems, economy, and progression

29. `[Q]` **System inventory**: "List every player-facing system (combat, crafting, dialogue, building, trading, stealth, and so on). For each, one line: what it does, and which pillar it serves. Anything that serves no pillar is a candidate to cut."
30. `[Q]` **Progression spine**: "How does the player get stronger, wider, or deeper over time: levels, gear, skills, unlocks, knowledge, story access? What is the pacing per hour of play?"
31. **Gating**: "Is progress gated by time, by skill, by content consumed, or by currency? Where do walls appear and are they intentional?"
32. **Resources**: "List the resources. For each: sources (where it comes from), sinks (where it goes), converters (what turns it into something else). Which resource is the scarce one that creates pressure?"
33. **Rewards**: "What reward types exist (power, cosmetic, narrative, social, informational)? What is the reward cadence in the first hour versus hour ten?"
34. **Dominant strategies**: "What is the strategy you expect players to find that trivializes the game, and what counters it?"
35. **System interactions**: "Name two pairs of systems that feed each other, and one pair you must keep apart."
36. `[C]` **Numbers**: "For your most important system, give me the starting values, the end-game values, and the curve shape between them (linear, exponential, stepped)."
37. `[C]` **Prestige and endgame**: "What happens when the progression spine ends? New game plus, prestige, endless mode, nothing?"
38. `[C]` **AI**: "What AI is needed: enemy behaviours, companion behaviours, director or pacing AI, procedural systems? What is the smartest thing an enemy does, and what is it never allowed to do?"

Exit check: system inventory with pillar mapping; progression spine and pacing; at least one resource with sources and sinks; dominant strategy risk named.

### Module 06 - Difficulty, onboarding, and retention

39. `[Q]` **First two minutes**: "Second by second, what happens from launch to first meaningful choice? What must the player understand by minute two, minute ten, hour one?"
40. `[Q]` **Difficulty model**: fixed, selectable presets, dynamic adjustment, assist toggles, player-authored (modifiers). "Which, and why does that fit the pillars?"
41. **Curve shape**: "Describe the difficulty curve across the main path in three or four beats. Where are the deliberate spikes and breathers?"
42. **Difficulty parameters**: "What are the atomic knobs that make a challenge harder or easier (enemy count, speed, timing window, resource scarcity, information)? Which do you turn first?"
43. **Teaching**: "For each core mechanic: how is it taught, tested, and then twisted? Are tutorials interactive and skippable?"
44. **Retention hooks**: "At the end of session one, what short, medium, and long goals can the player see? What brings them back tomorrow?"
45. **Failure and forgiveness**: "How punishing is failure (checkpoints, lives, permadeath, resource loss)? What is the forgiveness budget (assist modes, retries, hints)?"
46. `[C]` **Mastery ceiling**: "What does a player at hour fifty do that a player at hour one cannot? Is there a visible skill expression?"
47. `[C]` **Drop-off points**: "Where do you predict players quit, and what do you do about each?"

Exit check: first-two-minutes described; difficulty model chosen with rationale; teaching approach for core mechanics; at least one retention hook.

### Module 07 - Content structure and level design

48. `[Q]` **Structure**: linear, hub and spoke, open world, procedural, level select, roguelike runs, episodic. "Which, and how does that serve the loop?"
49. `[Q]` **Content volume**: "Give the table: areas or levels, enemy types, bosses or set pieces, items or weapons, biomes, side content. Numbers or ranges."
50. **Critical path and pacing**: "For a typical level or area: what is the critical path, what is the golden path (the ideal route), what are the beats, and how long does it take?"
51. **Level design principles**: "Three rules every level must follow, and one thing no level may do."
52. **Variety engine**: "What makes level fifteen feel different from level five: new verbs, new enemies, new rules, new setting, escalating combinations?"
53. **Replayability**: "What is replay built on: procedural generation, builds, difficulty tiers, alternate routes, new game plus, social? How long until a player has seen everything?"
54. **Save model**: "Save anywhere, checkpoints, autosave only, permadeath, run-based? What persists across runs or sessions, and what is deliberately lost?"
55. `[C]` **Procedural rules** (if procedural): "What is hand-authored versus generated, what guarantees does generation make (solvable, fair, varied), and what is the seed policy?"
56. `[C]` **Optional and secret content**: "How much content is optional, how is it signposted, and what rewards it?"

Exit check: structure chosen; content volume table has numbers or explicit unknowns; save model chosen; replay basis stated.

### Module 08 - Narrative, characters, and world (Conditional)

57. `[Q]` **Story weight**: "Is story the spine, a frame, or flavour? What percentage of play time is story delivery?"
58. `[Q]` **Premise and protagonist**: "One paragraph: setting, protagonist, what they want, what stands in the way, what is at stake."
59. **Delivery**: "How is story delivered: cutscenes, in-engine dialogue, environmental storytelling, item text, systemic events, barks? Which is primary, and what is mandatory versus optional?"
60. **Agency**: "Do choices change the story? Branch, flavour, or ending variations? How do you show the player their choice mattered?"
61. **Cast**: "List the named characters with a one-line role each. Who changes over the story?"
62. **World rules**: "What must be true about the world for the mechanics to make sense (why does the player respawn, why can they carry fifty swords, why do enemies drop money)?"
63. `[C]` **Tone guardrails**: "Three things the writing must always do and three it must never do."
64. `[C]` **Story bible need**: "Is lore deep enough to need a separate bible, and who owns consistency?"
65. `[C]` **Voice and text volume**: "Estimated word count, voiced lines, languages for VO?"

Exit check: story weight decided; premise stated; delivery method chosen; agency model chosen.

### Module 09 - Art, audio, and presentation

66. `[Q]` **Art direction in one sentence**: "Style, fidelity, and two or three visual references (games, films, artists). What single image would go on the store page?"
67. `[Q]` **Constraints shaping style**: "Which constraints drive the style: team size, performance targets, readability needs, art skills available?"
68. **Readability rules**: "What must the player always be able to read at a glance (enemies, hazards, interactables, allies)? How does the style guarantee it (silhouette, colour, light)?"
69. **Palette and mood**: "Palette in adjectives; how does it shift across the game (biomes, story acts, danger)?"
70. **Animation priorities**: "Which animations matter most for feel and readability, and what is the animation budget style (hand-keyed, mocap, procedural, minimal)?"
71. **Music direction**: "Genre, instrumentation, adaptive or linear, references. When is there no music?"
72. **Sound as information**: "Which sounds carry gameplay information the player needs, and what is the visual redundancy for each?"
73. `[C]` **VO plan**: "Full VO, partial, barks only, none? Languages?"
74. `[C]` **Technical art**: "Any rendering feature that defines the look (cel shading, pixel grid, PBR, volumetrics) and its performance cost?"

Exit check: art direction sentence with references; readability rules; music direction; sound-information mapping started.

### Module 10 - UI and UX

75. `[Q]` **Screen inventory**: "List every screen (title, menus, HUD, inventory, map, settings, results). Draw the flow in words: what leads where."
76. `[Q]` **HUD**: "What is on the HUD at all times, what is contextual, and what is diegetic (in the world) versus overlay? What could you remove?"
77. **Information hierarchy**: "In the busiest moment of play, what is the one thing the player must see first, then second, then third?"
78. **Input parity**: "Can every screen be operated with the same input as gameplay (gamepad-only, touch-only)? Where would mouse-only or keyboard-only creep in?"
79. **Settings**: "Which settings exist at launch (video, audio sliders, remapping, accessibility, language)? All must persist; anything that cannot?"
80. `[C]` **Feedback and errors**: "How does the UI confirm actions, warn before destructive ones, and recover from errors (XAG 115)?"
81. `[C]` **UI style**: "Is the UI part of the fiction (diegetic, skeuomorphic) or clean and neutral? Reference?"

Exit check: screen list; HUD contents; input parity answered.

### Module 11 - Multiplayer, online, and social (Conditional)

82. `[Q]` **Modes**: solo, local co-op, online co-op, PvP, asynchronous, shared world. "Which ship at launch, and which is the game designed around?"
83. `[Q]` **Session shape**: "Players per session, session length, drop-in/drop-out, host migration?"
84. **Authority model**: "Server-authoritative, peer-to-peer lockstep, rollback, relay? What is the latency budget the design tolerates, and which mechanics are most latency-sensitive?"
85. **Matchmaking and progression**: "Ranked, casual, both? Skill-based matching? Does progression differ by mode?"
86. **Cross-play and cross-progression**: "Required, desired, or excluded? What does it constrain?"
87. **Communication and safety**: "Voice, text, pings, emotes? Who moderates, with what tools, and what is the reporting flow? What age policy applies?"
88. **Anti-cheat posture**: "What does cheating look like here, how much does it matter (competitive integrity, economy), and what is the plan (server validation, third-party anti-cheat, none)?"
89. **UGC and sharing**: "Can players create, share, or trade content? Moderation and ownership rules?"
90. `[C]` **Social systems**: "Friends, guilds, leaderboards, spectating, streaming integration? Core or optional?"
91. `[C]` **Live-service posture**: "Seasons, events, battle pass, cadence? What is the minimum content velocity to sustain it?"

Exit check: modes at launch; session shape; authority model chosen or open; moderation and anti-cheat posture stated.

### Module 12 - Accessibility and localization

92. `[Q]` **Accessibility scope**: "Which of these are in at launch: full input remapping, text size and contrast options, colour never the sole signal, subtitles and captions for all speech, separate volume sliders, difficulty or assist options, reduced motion, screen reader for menus? Which are explicitly out, and why?"
93. **Design-level accessibility**: "Which mechanics are hardest to make accessible (timing windows, colour matching, audio cues, fine motor input) and what is the alternative for each?"
94. `[Q]` **Languages**: "Launch languages, later languages? Is any text baked into images or audio?"
95. **Localization readiness**: "Are strings externalized from day one, is the UI designed for thirty percent text expansion, will you pseudo-localize, and who maintains the glossary?"
96. `[C]` **Cultural review**: "Any content that needs regional variants or review (symbols, gestures, history, religion, gambling mechanics)?"
97. `[C]` **Certification and legal**: "Which platform certification requirements touch design (controller disconnect, suspend/resume, naming rules, privacy, trophies)? Any licensed IP, music, fonts, likeness, or open-source obligations?"

Exit check: accessibility in/out list; launch languages; strings externalized answer.

### Module 13 - Technical foundations

98. `[Q]` **Engine and rationale**: "Engine or framework, and why (team skill, platform reach, licensing, existing code)?"
99. `[Q]` **Performance targets**: "Per platform: frame rate, resolution, load-time budget, minimum spec. Which target is non-negotiable?"
100. **Technical unknowns**: "What are the two or three things you do not yet know how to build, and what prototype answers each first?"
101. **Scale drivers**: "What pushes the technology hardest: world size, entity count, simulation depth, streaming, physics, networking, procedural generation?"
102. **Persistence**: "What does the save system persist, what is the save format and size, how are corruption and version migration handled, cloud sync?"
103. **Telemetry**: "What is the single KPI that tells you the game is working (D1 retention, session length, completion rate, conversion)? First event buckets: session lifecycle, onboarding milestones, core loop events, monetization touchpoints, errors. Any privacy constraints?"
104. `[C]` **Tools**: "What internal tools does the team need (level editor, tuning sheets, debug cheats, replay, telemetry dashboards)?"
105. `[C]` **Build and release**: "Update cadence, patch policy, versioning of saves and content, platform submission lead times?"

Exit check: engine with rationale; performance targets per platform; top technical unknowns with prototype plan.

### Module 14 - Production, business, and risk

106. `[Q]` **Team and gaps**: "Confirm team size and roles from the concept stage. Which missing skill is the most dangerous gap, and how do you fill it (hire, contract, learn, cut)?"
107. `[Q]` **Milestones**: "Name the milestones you will use: prototype, first playable, vertical slice, alpha, beta, cert, launch. What is the exit criterion for the vertical slice, and what is in it?"
108. `[Q]` **Top risks**: "Give me the top five risks across design (is it fun?), technical, market, and team. For each: the cheapest test that reduces it, and what you do if the test fails."
109. **Riskiest assumption first**: "Which assumption, if wrong, kills the project? What is the smallest prototype that tests it, and when?"
110. **Success metrics**: "What numbers say it worked at launch: units, wishlists, review score, D1/D7/D30, completion rate, revenue? And what number says stop?"
111. **Budget**: "Confirm budget range. Does it cover salaries, tools, contracting, marketing, porting, cert, and a contingency?"
112. **Marketing and community**: "Who is the first thousand players and where are they? Demo, festivals, streamers, storefront events, Discord, devlogs?"
113. **Post-launch**: "What is the post-launch plan: patches only, DLC, seasons, ports, nothing? For how long?"
114. `[C]` **Playtesting plan**: "Who tests, how often, at which milestones, and what do you measure (task completion, fun ratings, retention proxies, confusion points)?"
115. `[C]` **Fun gate**: "How will the team know the core loop is fun enough to enter production? What is the evidence bar?"
116. `[C]` **Kill criteria**: "What would make you cancel or pivot?"
117. `[C]` **Document ownership**: "Who owns this GDD, and how often is it reviewed?"

Exit check: milestones with vertical slice criterion; five risks with tests; success metrics; riskiest assumption and its prototype.

### Module 15 - Genre probes (Comprehensive-only)

Run the set that matches `gameInfo.genre`; run two sets for hybrids. Skip any probe already answered.

**Action / shooter / brawler**
- Enemy roster by role (fodder, pressure, ranged, elite, boss) and the counter each teaches
- Weapon or ability roster and what makes each situationally best
- Encounter design rules: arena shapes, spawn logic, escalation
- Health, healing, and resource economy in combat
- Stagger, i-frames, parry, dodge windows in frames or milliseconds

**RPG / action RPG**
- Character build axes (class, stats, gear, skills) and how many viable builds at endgame
- Loot philosophy: rarity tiers, drop rates, crafting versus finding
- Party or companion system and AI control
- Quest structure: main, side, emergent; quest volume
- Dialogue system and choice consequences

**Strategy / 4X / tactics**
- Unit or faction roster and asymmetry
- Economy layers and tech tree shape
- Fog of war and information
- Turn or tick structure; simultaneous or sequential
- AI opponent behaviour and difficulty levers
- Map generation and scenario design

**Simulation / management / tycoon / builder**
- Simulated entities and their needs; what happens when needs are unmet
- Growth curve and the failure spiral guard
- Automation versus manual play over time
- Sandbox versus scenario versus campaign structure
- Information UI: how players diagnose problems

**Puzzle**
- Puzzle grammar: the rules, the number of mechanics, how they combine
- Level authoring process and hint system
- Difficulty ramp and the "eureka" budget per hour
- Fail state and undo policy

**Platformer**
- Jump physics parameters and forgiveness windows
- Movement upgrade order and what each unlocks in level design
- Death and checkpoint density
- Collectibles and their purpose

**Roguelike / roguelite**
- Run length, run structure, and biomes
- Meta-progression: what persists and what it must never make trivial
- Item and synergy count; how synergies are discovered
- Seed and daily-run policy
- Unlock pacing across runs

**Survival / crafting / open world**
- Survival meters and their pressure curves
- Crafting tree depth and material sources
- Base building rules and persistence
- World size, points of interest density, travel time
- Threat escalation over time

**Horror**
- The fear model: dread, jump, chase, resource scarcity, helplessness
- Safety and tension rhythm
- Enemy visibility and knowledge rules
- Player power curve (do they ever get stronger?)

**Narrative / adventure / visual novel**
- Branch structure and the branch budget
- Choice presentation and timing
- Puzzle or interaction density between story beats
- Endings and what determines them

**Racing / sports / vehicle**
- Handling model and assist options
- Track or venue roster and progression
- AI competitor behaviour and rubber-banding policy
- Career structure and customization

**Fighting**
- Roster size, archetypes, and the universal mechanics
- Frame data philosophy, input complexity, execution barrier
- Single-player content
- Netcode requirements (rollback expected)

**Rhythm / music**
- Input timing windows and calibration
- Song count, licensing, difficulty tiers
- Note or pattern authoring process

**Idle / incremental / mobile casual**
- Session length targets and offline progress
- Prestige loop
- Monetization touchpoints and their fairness rules
- Notification and re-engagement policy

**MMO / shared world**
- Shard model and population targets
- Endgame loop and content velocity
- Economy controls (inflation, trading, bind rules)
- Social structure (guilds, raids, housing)

**Sandbox / creative / UGC-first**
- Building grammar and constraints
- Sharing and discovery of creations
- Moderation and ownership

**Party / local multiplayer**
- Minigame count and rotation
- Teach time per game
- Catch-up and chaos balance
- Spectator fun

**Educational / serious**
- Learning objectives and how play evidences them
- Assessment model
- Stakeholder constraints (curriculum, compliance)

### Module 99 - Closing

Run the protocol's closing sequence: coverage summary, overrule assumptions, "the one thing this document must get right", "what worries you most", "what did I not ask".

## Output: `details.md`

Write `details.md` as the synthesized narrative of the ledger. Every claim must trace to a ledger row; cite row IDs in parentheses where a reader might want to check. Keep it to what was actually decided; do not pad missing modules with generic text - say "not covered at this depth" and list the questions as open.

```markdown
# Design Details: [Project Name]

Interview: interview-video-game, depth [quick | standard | comprehensive]
Modules completed: [n] of [planned]
Ledger: [decided] decided, [assumed] assumed, [open] open, [skipped] skipped

## 1. Vision and scope
Pillars (confirmed): ...
Scope table: | Dimension | Count | Confidence |
MVP boundary: ...
Aha moment: ...

## 2. Platform, input, and business
Platform order, primary input, business model and price, rating target, platform features, distribution.

## 3. Core loop and verbs
Verb table: | Verb | Core/secondary | Input | Notes |
Loops: 30 s / 5 min / session / meta.
Tension, objectives, fail states and cost, feedback signals, meaningful choices, randomness map.

## 4. Character, camera, controls, and game feel
(or "Not applicable: [reason]")

## 5. Systems, economy, and progression
System inventory: | System | Purpose | Pillar served | Interacts with |
Progression spine and pacing; gating; resources: | Resource | Sources | Sinks | Converters | Pressure |
Rewards, dominant strategy risks, AI needs.

## 6. Difficulty, onboarding, and retention
First two minutes; understanding by minute two / ten / hour one; difficulty model and rationale; curve beats; difficulty parameters; teaching plan per core mechanic; retention hooks; failure and forgiveness.

## 7. Content structure and level design
Structure; content volume table; critical and golden path; level rules; variety engine; replayability; save model; procedural rules; optional content.

## 8. Narrative, characters, and world
(or "Not applicable: [reason]")

## 9. Art, audio, and presentation
Art direction sentence and references; constraints; readability rules; palette; animation priorities; music; sound-information table: | Information | Sound | Visual redundancy |; VO; technical art.

## 10. UI and UX
Screen list and flow; HUD (always / contextual / diegetic); information hierarchy; input parity; settings; feedback and error rules; UI style.

## 11. Multiplayer, online, and social
(or "Not applicable: [reason]")

## 12. Accessibility and localization
In-scope and out-of-scope accessibility features with reasons; hard mechanics and alternatives; languages; localization readiness; cultural review; certification and legal notes.

## 13. Technical foundations
Engine and rationale; performance targets per platform; technical unknowns and prototypes; scale drivers; persistence; telemetry KPI and event buckets; tools; release policy.

## 14. Production, business, and risk
Team and gaps; milestones with vertical slice criterion; risk table: | Risk | Type | Test | If it fails |; riskiest assumption and prototype; success and stop metrics; budget coverage; marketing and community; post-launch; playtesting plan; fun gate; kill criteria; document owner.

## 15. Genre-specific notes
(comprehensive depth only)

## Assumptions
| Ledger ID | Assumption | Rationale | Overrule by |

## Open questions
| Ledger ID | Question | Why it matters | Likely resolved by (research / prototype / playtest / decision) |

## Closing answers
The one thing to get right; biggest worry; things not asked.

---
*Generated by Video Game Interview*
*Date: [ISO timestamp]*
```

## Behaviors

1. Rephrase every bank question in the language of this game; never read the bank verbatim.
2. Pillars are the tie-breaker; name them when the designer is torn.
3. Numbers over adjectives. When the designer gives an adjective, ask for the number once, then accept "open".
4. Do not let the interview become a lecture. If you find yourself explaining a concept for more than two sentences, ask the question instead.
5. Write the ledger after every module. An interrupted interview must be resumable at the next module with nothing lost.
