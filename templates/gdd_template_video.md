# GDD Template - Video Game

Structure for Game Design Documents written by the `gdd-writer` role for the `video` family. Section levels come from `docs/gdd-best-practices.md` section 2:

- **Core** sections are always present. If the ledger has no material for one, the section states what is decided, marks the rest `[Open: ID]`, and stays short.
- **Recommended** sections are present unless the writer adds a one-line "Not applicable: [reason]" note under the heading.
- **Optional** sections are included only when the ledger has material for them; omit silently otherwise.

Formatting rules: each H2 section opens with a one- to three-sentence summary in bold; tables for anything with more than three comparable items; every system names the pillar it serves; inline `[Assumption: ID]` and `[Open: ID]` tags where a claim rests on an assumed or open ledger row; no marketing adjectives; no restating the heading.

---

```markdown
# [Game Title] - Game Design Document

| | |
|---|---|
| **Logline** | [One sentence] |
| **Version** | 1.0 |
| **Status** | Draft |
| **Date** | [ISO date] |
| **Owner** | [Designer / role; "unassigned" is acceptable and appears in Open Questions] |
| **Family / type** | Video game / [PC-console, mobile, VR, web] |
| **Genre** | [Primary; secondary] |
| **Platforms** | [Order] |
| **Players** | [Count and modes] |
| **Business model** | [Premium at price / F2P / not commercial] |
| **Rating target** | [Target or Open] |

---

## Contents

[Numbered list of H2 sections actually present]

---

## 1. Vision (one page)

**[Summary]**

### 1.1 Pitch
[Two or three sentences. This block must make sense with nothing else read.]

### 1.2 Design pillars
| # | Pillar | Meaning | What it makes us cut |
|---|--------|---------|----------------------|
| 1 | [Pillar] | [One line] | [Refused feature or behaviour] |

### 1.3 What this game is not
- [Exclusion] - [why]
- **Avoid resembling**: [anti-reference and why]

### 1.4 Player fantasy and experience goals
- **Fantasy**: [who the player gets to be / do]
- **Experience goals**: [feeling] when [moment]; [feeling] when [moment]
- **Best moment**: [concrete description from the interview]
- **Aha moment**: [what it is and when it lands]

### 1.5 Audience
- **Ideal player**: [...]
- **Not for**: [...]

### 1.6 Comparables
| Game | Keep | Change |
|------|------|--------|

### 1.7 Unique selling points
1. [USP]

### 1.8 Key features
- [Feature - one line each, five to eight]

### 1.9 Scope
| Dimension | Target | Confidence |
|-----------|--------|------------|
| Main path hours | | |
| Levels / areas | | |
| Enemy or challenge types | | |
| Player-facing systems | | |
| Named characters | | |

**MVP boundary**: [what ships if time is cut by three quarters]

### 1.10 Game flow summary
[One paragraph: a session from launch to quit, then the arc across the whole game.]

---

## 2. Core Gameplay

**[Summary]**

### 2.1 Core loop
| Timescale | Loop | What closes it |
|-----------|------|----------------|
| 30 seconds | | |
| 5 minutes | | |
| Session | | |
| Meta | | |

```
[Optional text diagram of the primary loop]
```

### 2.2 Player verbs
| Verb | Core / secondary | Input | Serves pillar | Notes |
|------|------------------|-------|---------------|-------|

### 2.3 Central tension and meaningful choice
[The tension the player manages, why it is fun, two example decisions with the information available when choosing.]

### 2.4 Objectives, win, and failure
[Objectives per timescale; fail states; what failure costs; time back into play.]

### 2.5 Feedback and readability
[The signals that tell the player how they are doing.]

### 2.6 Randomness
| Where | Input / output | Purpose |
|-------|----------------|---------|

---

## 3. Character, Camera, Controls, and Game Feel
*Core for real-time games; "Not applicable" for turn-based or menu-driven games.*

**[Summary]**

- **Perspective and camera**: [model; who controls it; idle behaviour]
- **Movement model**: [speed, acceleration, jump, weight, closest reference]
- **Control map**: | Verb | Primary input | Alternate | Remappable |
- **The one interaction that must feel great**: [which, and what "great" means]
- **Feedback techniques in scope**: [hit-stop, shake, particles, rumble ...] and refused
- **Response targets**: [latency, cancel rules, forgiveness windows] or `[Open: ID]`

---

## 4. Systems

**[Summary: how the systems fit together and which pillar each serves.]**

### 4.1 System inventory
| System | Purpose | Serves pillar | Feeds | Fed by |
|--------|---------|---------------|-------|--------|

### 4.2 [System name]
- **Purpose**: 
- **Rules**: [how it works, concretely]
- **Player interaction**: 
- **Tuning values**: [start, end, curve] or `[Open: ID]`
- **Interactions**: [systems it feeds or is fed by]
- **Risks**: [dominant strategy, degenerate case]

*(Repeat 4.x per system. Keep each under a page; deeper detail goes to a supplement.)*

### 4.x Systems that must stay apart
[Pairs deliberately kept from interacting, and why.]

### 4.y AI
[Enemy, companion, director, procedural. The smartest thing an enemy does; what it is never allowed to do.] or "Not applicable".

---

## 5. Economy and Progression

**[Summary]**

### 5.1 Resources
| Resource | Sources | Sinks | Converters | Pressure role |
|----------|---------|-------|------------|---------------|

### 5.2 Progression spine
[Type; pacing per hour; gating (time, skill, content, currency); milestones table.]

| Milestone | Trigger | Unlocks | Approx. hour |
|-----------|---------|---------|--------------|

### 5.3 Rewards
[Reward types and cadence, hour one versus hour ten.]

### 5.4 Endgame and prestige
[What happens when the spine ends.] or "Not applicable".

### 5.5 Monetization touchpoints
*Only when the business model is not premium.* [What is sold, currencies, the thing never sold, fairness rules.]

---

## 6. Difficulty, Onboarding, and Retention

**[Summary]**

### 6.1 First two minutes
[Second-by-second from launch to first meaningful choice.]

### 6.2 What the player understands by
| Minute 2 | Minute 10 | Hour 1 |
|----------|-----------|--------|

### 6.3 Difficulty model
[Fixed / presets / dynamic / assists / modifiers, with rationale tied to pillars.]

### 6.4 Difficulty curve and parameters
[Curve in three or four beats; spikes and breathers; the atomic knobs and which turn first.]

### 6.5 Teaching plan
| Mechanic | Taught by | Tested by | Twisted by |
|----------|-----------|-----------|------------|

### 6.6 Failure and forgiveness
[Punishment model; forgiveness budget.]

### 6.7 Retention hooks
[Short, medium, long goals visible at end of session one; predicted drop-off points and responses.]

---

## 7. Content and Level Design

**[Summary]**

### 7.1 Structure
[Linear / hub / open / procedural / runs; why it serves the loop.]

### 7.2 Content volume
| Content type | Count | Confidence |
|--------------|-------|------------|

### 7.3 Level design principles
[Three rules every level follows; one thing no level may do; critical and golden path definitions; beats and pacing for a typical level.]

### 7.4 Variety engine
[What makes late content feel different from early content.]

### 7.5 Replayability
[Basis; time until a player has seen everything.]

### 7.6 Save model
[Save anywhere / checkpoints / autosave / permadeath / runs; what persists; what is deliberately lost.]

### 7.7 Procedural generation
*Optional.* [Authored versus generated; guarantees; seed policy.]

### 7.8 Optional and secret content
*Optional.*

---

## 8. Narrative, Characters, and World
*Recommended; Core for narrative games; "Not applicable" when the ledger has no story.*

**[Summary]**

- **Story weight**: [spine / frame / flavour; share of play time]
- **Premise**: [setting, protagonist, want, obstacle, stakes]
- **Delivery**: [primary and secondary methods; mandatory versus optional]
- **Agency**: [branch / flavour / endings; how choice is shown to matter]
- **Cast**: | Character | Role | Arc |
- **World rules**: [what must be true for the mechanics to make sense]
- **Tone guardrails**: [always / never]
- **Story bible**: [needed or not; owner]

---

## 9. Art, Audio, and Presentation

**[Summary]**

### 9.1 Art direction
[One sentence; references; the store-page image; constraints that shape the style.]

### 9.2 Readability rules
[What the player must always read at a glance and how the style guarantees it.]

### 9.3 Palette and mood
[Palette; how it shifts across the game.]

### 9.4 Animation priorities
[Which animations matter most; production style.]

### 9.5 Music direction
[Genre, instrumentation, adaptive or linear, references, silence.]

### 9.6 Sound as information
| Information | Sound | Visual redundancy |
|-------------|-------|-------------------|

### 9.7 Voice
[Plan and languages.] or "None".

---

## 10. UI and UX

**[Summary]**

### 10.1 Screen inventory and flow
[List of screens; flow in words or a text diagram.]

### 10.2 HUD
| Element | Always / contextual | Diegetic / overlay | Purpose |
|---------|---------------------|--------------------|---------|

### 10.3 Information hierarchy
[In the busiest moment: first, second, third thing seen.]

### 10.4 Input parity and settings
[Every screen operable with the gameplay input; settings list; persistence.]

### 10.5 Feedback and error handling
[Confirmation, destructive-action warnings, recovery.]

---

## 11. Multiplayer, Online, and Social
*Core if multiplayer; otherwise "Not applicable".*

**[Summary]**

- **Modes at launch**: 
- **Session shape**: [players, length, drop-in, host migration]
- **Authority model**: [server / lockstep / rollback / relay; latency budget; latency-sensitive mechanics]
- **Matchmaking and progression**: 
- **Cross-play / cross-progression**: 
- **Communication and safety**: [channels, moderation, reporting, age policy]
- **Anti-cheat posture**: 
- **UGC and sharing**: 
- **Social systems**: 
- **Live-service posture**: [cadence, minimum content velocity] or "None"

---

## 12. Accessibility and Localization

**[Summary]**

### 12.1 Accessibility scope
| Feature | In / out at launch | Reason |
|---------|--------------------|--------|
| Full input remapping | | |
| Text size and contrast options | | |
| Colour never the sole signal | | |
| Subtitles and captions | | |
| Separate volume sliders | | |
| Difficulty or assist options | | |
| Reduced motion | | |
| Menu screen reader | | |

### 12.2 Hard mechanics and alternatives
| Mechanic | Barrier | Alternative |
|----------|---------|-------------|

### 12.3 Localization
[Launch and later languages; strings externalized; 30% expansion; pseudo-localization; glossary owner; text in images or audio.]

### 12.4 Cultural review, certification, and legal
*Optional.* [Regional variants; certification constraints touching design; IP, music, fonts, likeness, open-source obligations.]

---

## 13. Technical Foundations

**[Summary]**

### 13.1 Engine
[Choice and rationale.]

### 13.2 Performance targets
| Platform | Frame rate | Resolution | Load budget | Min spec | Non-negotiable? |
|----------|------------|------------|-------------|----------|-----------------|

### 13.3 Technical unknowns
| Unknown | Prototype that answers it | When |
|---------|---------------------------|------|

### 13.4 Scale drivers
[What pushes the technology hardest.]

### 13.5 Persistence
[What is saved; format; corruption and migration; cloud.]

### 13.6 Telemetry
[Top KPI; first event buckets; privacy constraints.] or "None planned".

### 13.7 Tools and release
*Optional.* [Internal tools; update cadence; save and content versioning; submission lead times.]

---

## 14. Production, Business, and Risk

**[Summary]**

### 14.1 Team
| Role | Who | Status (in place / gap / plan) |
|------|-----|--------------------------------|

### 14.2 Milestones
| Milestone | Contents | Exit criterion | Target |
|-----------|----------|----------------|--------|
| Prototype | | | |
| First playable / vertical slice | | | |
| Alpha | | | |
| Beta | | | |
| Cert / launch | | | |

### 14.3 Risks
| # | Risk | Type | Cheapest test | If it fails |
|---|------|------|---------------|-------------|

**Riskiest assumption**: [what, the prototype that tests it, when]

### 14.4 Success and stop metrics
[Launch metrics; the number that says stop.]

### 14.5 Budget
[Range; coverage of salaries, tools, contracting, marketing, porting, cert, contingency.] or "Not stated".

### 14.6 Marketing and community
*Optional; Core for commercial.* [First thousand players; channels; demo and events.]

### 14.7 Post-launch
[Patches, DLC, seasons, ports; duration.] or "None".

### 14.8 Playtesting plan and fun gate
[Who, when, what is measured; the evidence bar for entering production.]

### 14.9 Kill criteria and ownership
[What would cancel or pivot the project; document owner and review cadence.]

---

## 15. Market Positioning
*Optional; include for commercial projects with market material in the ledger or research.*

[Position; competitive advantages; channels; key messages. Keep pitch language out of the design sections above.]

---

## Appendices

### A. Decision log
| Date | Decision | Rationale | Ledger ID(s) | Supersedes |
|------|----------|-----------|--------------|------------|

### B. Assumptions
| Ledger ID | Assumption | Rationale | Overrule by |
|-----------|------------|-----------|-------------|

### C. Open questions
| Ledger ID | Question | Why it matters | Resolved by | Owner |
|-----------|----------|----------------|-------------|-------|

### D. Glossary
| Term | Definition |
|------|------------|

### E. Research notes
*Only when research_synthesis.md exists.* [Findings that shaped decisions, with confidence levels; what remains unverified.]

### F. Document history
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [date] | Game Doc Forge | Initial draft from interview ledger |

---
*Generated by Game Doc Forge*
*[ISO timestamp]*
```
