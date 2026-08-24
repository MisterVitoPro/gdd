# GDD Template - Tabletop

Structure for Game Design Documents written by the `gdd-writer` role for the `tabletop` family (board, card, dice, party, miniatures, and tabletop RPG designs). Section levels come from `docs/gdd-best-practices.md` section 3:

- **Core** sections are always present. If the ledger has no material for one, the section states what is decided, marks the rest `[Open: ID]`, and stays short.
- **Recommended** sections are present unless the writer adds a one-line "Not applicable: [reason]" note under the heading.
- **Optional** sections are included only when the ledger has material for them; omit silently otherwise.
- **TTRPG** sections (14-15) are Core when `gameInfo.type` is `ttrpg` and omitted otherwise.

Formatting rules: each H2 section opens with a one- to three-sentence summary in bold; tables for anything with more than three comparable items; every mechanism names the pillar it serves; inline `[Assumption: ID]` and `[Open: ID]` tags where a claim rests on an assumed or open ledger row; playtest-dependent claims say so; no marketing adjectives; no restating the heading.

---

```markdown
# [Game Title] - Game Design Document

| | |
|---|---|
| **Hook** | [One sentence] |
| **Version** | 1.0 |
| **Status** | Draft |
| **Date** | [ISO date] |
| **Owner** | [Designer; "unassigned" appears in Open Questions] |
| **Type** | [Board / card / dice / party / miniatures / TTRPG / hybrid] |
| **Players** | [Range (best count)] |
| **Time** | [First play / experienced] |
| **Age** | [Minimum] |
| **Weight** | [BGG 1-5 target] |
| **Route** | [Self-publish / crowdfund / pitch / print-and-play / hobby] |
| **Target price** | [MSRP or Open] |

---

## Contents

[Numbered list of H2 sections actually present]

---

## 1. Vision (one page)

**[Summary]**

### 1.1 Pitch
[Two or three sentences. Must stand alone.]

### 1.2 Design pillars
| # | Pillar | Meaning | What it makes us cut |
|---|--------|---------|----------------------|

### 1.3 What this game is not
- [Exclusion] - [why]
- **Avoid resembling**: [anti-reference and why]

### 1.4 Core experience
[Where the fun is and for whom; feelings and when they peak; what players say during and after.]

### 1.5 Audience and weight
- **Ideal player**: [tier: family, gateway, hobby, heavy; what they play now]
- **Not for**: [...]
- **Weight justification**: [rules count, choices per turn, bookkeeping, luck]

### 1.6 Shelf neighbours
| Game | Familiar (kept) | New (changed) |
|------|-----------------|---------------|

### 1.7 Hooks
1. [Hook a publisher hears in the first thirty seconds]

### 1.8 Scope
| Dimension | Target | Confidence |
|-----------|--------|------------|
| Components (total pieces) | | |
| Cards | | |
| Scenarios / maps / modes | | |
| Rulebook pages | | |

**MVP prototype**: [the smallest build that tests the heart mechanism and the question it answers]

---

## 2. Theme and Player Role

**[Summary]**

- **Role and goals**: [who a player is; individual goal; collective goal]
- **Theme strength**: [reason for being / integrated / coat of paint]
- **Setting and tone**: 
- **Mechanism-theme fit**:

| Mechanism | In-fiction reason | Where it breaks |
|-----------|-------------------|-----------------|

- **Narrative arc**: *Optional.* [opening, crisis, resolution on the table]
- **Sensitivity notes**: *Optional.* [real-world subjects needing accuracy or care]

---

## 3. Mechanisms and Turn Structure

**[Summary: the heart mechanism and its supporters, each with the pillar it serves.]**

### 3.1 Mechanisms
| Mechanism | Heart / support | Serves pillar | Notes |
|-----------|-----------------|---------------|-------|

### 3.2 A turn, step by step
1. [Step]
2. ...

### 3.3 Round and phase structure
[Turn order model; phases; what resets and what carries.]

### 3.4 Actions
| Action | Cost | Effect | Expected frequency | Notes |
|--------|------|--------|--------------------|-------|

### 3.5 Information
[Hidden from whom and why; public; bluffing or deduction.]

### 3.6 Game arc
[Opening / midgame / endgame texture and what changes it.]

### 3.7 Simplification candidate
[The rule cut first if the game is too long or too heavy.]

---

## 4. Decision Space and Interaction

**[Summary]**

### 4.1 Decision points
| Decision | Typical options | Information available | Serves pillar |
|----------|-----------------|------------------------|---------------|

### 4.2 Central tension
[The trade-off at the heart; where it is felt most.]

### 4.3 Interaction model
[Direct / indirect / negotiation / cooperative / take-that / none; why it fits the audience.]

### 4.4 Reading, consequences, and recovery
[What watching others reveals; planning horizon; how recoverable mistakes are.]

### 4.5 Kingmaking and politics
[Can a losing player decide the winner; stance; limits.]

### 4.6 New-player legibility
[What a first-timer sees as a good play on turn one.]

### 4.7 Cooperative design
*Only for cooperative games.* [Quarterback prevention; private information; loss condition and expected loss rate.]

---

## 5. Economy, Scoring, and End Game

**[Summary]**

### 5.1 Resources
| Resource | Gained by | Spent on | Converts to | Pressure role |
|----------|-----------|----------|-------------|---------------|

### 5.2 End condition
[What ends the game; what drives it to conclusion so it cannot stall.]

### 5.3 Winning
[Determination; tiebreakers.]

### 5.4 Scoring shape
[Sources in rough proportion; during / end; hidden / public.]

### 5.5 Growth curve
[How capability grows; what caps it.]

### 5.6 Endgame phase
[Can players see the end coming; how play changes.]

### 5.7 Strategy paths and score spread
*Recommended.* [Distinct competitive strategies; typical winning score; desired spread.]

---

## 6. Balance, Pacing, and Luck

**[Summary. Mark playtest-dependent claims.]**

### 6.1 Catch-up and runaway leader
[Mechanism or stance; visible or invisible.]

### 6.2 Luck map
| Where | Input / output | Frequency | Purpose |
|-------|----------------|-----------|---------|

**New player beats veteran**: [target frequency]

### 6.3 Dominant strategies
| Feared strategy | Counter | Cost of countering |
|-----------------|---------|--------------------|

### 6.4 Start positions and turn order
[Symmetry; first or last player advantage; compensation.]

### 6.5 Elimination and downtime
[Elimination stance; wait times at max count.]

### 6.6 Win curve
[Leader's chance of winning versus lead; desired shape.]

### 6.7 Analysis paralysis
[Hotspots and limiters.]

### 6.8 Asymmetry and variability
*Optional.* [Balancing approach; acceptable win-rate band; what varies between plays.]

---

## 7. Player-Count Scaling and Solo
*Recommended when the count range spans more than two values or a solo mode exists; otherwise "Not applicable".*

**[Summary]**

| Count | Changes | Interaction level | Expected time |
|-------|---------|-------------------|---------------|

- **Two-player**: [special rules or dummy]
- **Solo mode**: [automa / beat-your-score / puzzle / campaign; what it replaces; rules overhead] or "None"
- **Component ceiling**: [enough for the full range?]

---

## 8. Cards and Decks
*Core when cards are a primary component; otherwise "Not applicable".*

**[Summary]**

- **Card anatomy**: [fields; maximum text length]
- **Card types and counts**:

| Type | Count | Deck (shared / personal / market / draft) |
|------|-------|-------------------------------------------|

- **Deck lifecycle**: [draw, hand size, discard, reshuffle, trash, limits, pressure]
- **Distribution curve**: [cost / type / power distribution; the card everyone wants and its limiter]
- **Synergies**: [count; discovery; feared degenerate combo]
- **Sheet math and format**: *Optional.* [multiples of printer sheet; size; finish; expansion and errata policy]

---

## 9. Components, Table, and Production

**[Summary]**

### 9.1 Component manifest
| Component | Count | Size / material | Rules work it does | Custom? |
|-----------|-------|-----------------|--------------------|---------|

### 9.2 Table footprint, setup, and teardown
[Space at max count; setup and teardown minutes; longest setup step.]

### 9.3 Box and shipping
[Box size and weight class; punchboard clearance; shipping category.]

### 9.4 Cost sanity
[Landed cost estimate at a plausible print run versus target price, using the ratios in the ledger; or `[Open: ID]`.]

### 9.5 Digital and print-and-play
*Optional.* [Tabletop Simulator, Tabletopia, BGA, PnP; what changes.]

### 9.6 Quality tiers
*Optional.* [Standard / deluxe; the deluxe upgrade and whether it changes play.]

---

## 10. Rules, Teach, and Reference

**[Summary]**

### 10.1 Teach script (under ten minutes)
1. [Order of explanation]

### 10.2 Rulebook outline
[Overview; components; setup; turn; actions; end and scoring; reference; estimated length; unusual sections.]

### 10.3 Reference aids
[Turn summary cards, icon glossary, scoring pad, on-board reminders.]

### 10.4 Known edge cases
| Situation | Ruling | Confidence |
|-----------|--------|------------|

### 10.5 First-play variant
[Simplified setup or rules for new tables.] or "None".

### 10.6 Rules review
*Optional.* [Editors; blind readers.]

---

## 11. Graphic Design and Accessibility

**[Summary]**

- **Readability floor**: [font sizes; what is read across the table]
- **Colour independence**: [colours never the sole signal; how]
- **Language independence**: [text-heavy or iconographic; localization plan]
- **Iconography**: [count; learnability; confusion risks]
- **Physical accessibility**: [component size; contrast; card holders; reach]
- **Art direction**: *Recommended.* [style; references; illustration count; art as information; AI-generation disclosure policy]
- **Ambiguity audit**: *Optional.* [board adjacency, text, symbols that could read two ways]

---

## 12. Prototyping and Playtesting

**[Summary]**

### 12.1 Current state
[What exists; plays so far; with whom.]

### 12.2 Prototype log
| Version | Date | Question it answers | Result |
|---------|------|---------------------|--------|

### 12.3 Playtest plan
| Stage | Sessions | Gate to next stage |
|-------|----------|--------------------|
| Solo and internal | | |
| Friends | | |
| Targeted strangers | | |
| Blind (no designer present) | | |
| Stress and convention | | |

### 12.4 Session metrics
[Fields logged per session.]

### 12.5 Post-play questions
[The questions asked at every table.]

### 12.6 Stop criterion
[The evidence bar for "done".]

### 12.7 Known problems
| Problem | Tried | Status |
|---------|-------|--------|

---

## 13. Publishing, Pricing, and Market

**[Summary]**

### 13.1 Route
[Self-publish / crowdfund / pitch / PnP / hobby; what decides it.]

### 13.2 Price point
[Target; justification against experience, audience, time, components.]

### 13.3 Target publishers or shelf
*Optional.* [Publishers on this shelf; facsimile check.]

### 13.4 Sell sheet
[Hook; three bullets; hero image description; component list; contact line.]

### 13.5 Pitch
*Optional.* [First-thirty-seconds hooks; video; digital table.]

### 13.6 Market evidence
[Ratings, convention interest, comparable campaigns, community.]

### 13.7 Crowdfunding
*Optional.* [Goal derivation; stretch goal policy; fulfilment regions; timeline.]

### 13.8 Rights and credits
*Optional.*

---

## 14. TTRPG: Resolution and Characters
*Core for TTRPGs; omitted otherwise.*

**[Summary]**

- **Core resolution**: [dice / cards / tokens / diceless; probability shape; partial success]
- **Character creation**: [attributes, skills, classes or playbooks, ancestry, backgrounds, equipment; time; the defining choice]
- **Advancement**: [XP / milestones / narrative; power curve]
- **Conflict**: [combat model and length; non-combat systems]
- **Consequences**: [damage, conditions, death, recovery; lethality]
- **Player-facing complexity**: [sheet contents; player versus GM rules load]
- **Design lineage**: *Optional.* [nearest systems; kept and rejected]
- **Safety tools**: [built-in tools and session zero guidance]

---

## 15. TTRPG: GM Tools, Setting, and Campaign
*Core for TTRPGs; omitted otherwise.*

**[Summary]**

- **GM prep and tools**: [prep per session; random tables, procedures, builders, clocks]
- **Setting delivery**: [pre-built / toolkit / generic; lore volume and presentation]
- **Session and campaign structure**: [procedures for exploration, downtime, travel, factions]
- **Challenge design**: [difficulty budget or rating]
- **Book structure**: [core book contents and pages; player and GM split; starter adventure]
- **Supporting products**: *Optional.*
- **Onboarding new groups**: *Optional.* [quickstart, actual play, organized play]

---

## 16. Campaign, Legacy, and Expansion
*Optional; include when the ledger has campaign, legacy, scenario, or expansion material.*

- **Structure**: [scenarios / campaign / legacy / modular; arc length]
- **Persistence**: [what carries; joining mid-campaign]
- **Replay after completion**: 
- **Content volume**: 
- **Expansion hooks**: [systems designed to accept expansions; what is deliberately withheld]

---

## Appendices

### A. Decision log
| Date | Decision | Rationale | Ledger ID(s) | Supersedes |
|------|----------|-----------|--------------|------------|

### B. Assumptions
| Ledger ID | Assumption | Rationale | Overrule by |
|-----------|------------|-----------|-------------|

### C. Open questions
| Ledger ID | Question | Why it matters | Resolved by (playtest / research / decision) | Owner |
|-----------|----------|----------------|----------------------------------------------|-------|

### D. Glossary
| Term | Definition |
|------|------------|

### E. Research notes
*Only when research_synthesis.md exists.*

### F. Document history
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [date] | Game Doc Forge | Initial draft from interview ledger |

---
*Generated by Game Doc Forge*
*[ISO timestamp]*
```
