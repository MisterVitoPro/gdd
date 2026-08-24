# Interview Protocol

Shared method for every interactive role in the pipeline (`concept-gatherer`, `interview-video-game`, `interview-tabletop`). Each role file defines *what* to ask; this file defines *how* to ask it, how answers are recorded, and how the interview ends. Read it once at the start of the interview and follow it throughout.

---

## 1. Principles

1. **One decision per question.** Never bundle two unrelated decisions into one prompt. Closely related multiple-choice questions may be batched (see section 4), but each still gets its own answer and ledger row.
2. **Ask, do not assume.** When the designer has not said it, it is not decided. If you must fill a gap to keep moving, record it as an *assumption* with your rationale, never as a fact.
3. **Concrete over abstract.** Prefer "What happens in the first 60 seconds?" to "Describe onboarding." Prefer "What is the player's verb list?" to "Describe the mechanics." Ask for numbers, names, and examples.
4. **Build on what is known.** Reference earlier answers and the concept pillars by name. Every question should feel like it belongs to *this* game, not a generic survey.
5. **Pillars are the tie-breaker.** When the designer is torn, ask which option serves the design pillars better. If an answer contradicts a pillar, say so and let the designer resolve it (change the answer or change the pillar).
6. **Offer examples when stuck.** If the designer hesitates, give two or three contrasting options drawn from comparable games, then let them choose or invent.
7. **Say what it is not.** Periodically ask what the game deliberately excludes. Exclusions are as valuable to a GDD as inclusions.
8. **Respect energy.** Long interviews fatigue people. Show progress, allow pausing, and never re-ask something already answered.

## 2. Depth levels

At the start of the game-type interview, ask the designer to pick a depth. Record it in `state.json` at `checkpoints.interviewDepth.depth` and `interview.depth`.

| Depth | Target | Coverage |
|-------|--------|----------|
| `quick` | 20-30 questions, ~15 minutes | Core modules only, one probe per topic, everything else assumed and flagged |
| `standard` | 45-70 questions, ~35 minutes | Core modules in full plus conditional modules that apply, secondary probes where answers are thin |
| `comprehensive` | 90+ questions, 60+ minutes | Every applicable module, every probe, plus genre-specific probes; the aim is to leave no section of the GDD resting on an assumption |

Each role file tags its modules as **Core** (always asked), **Conditional** (asked when a trigger in `concept.md` or an earlier answer applies), or **Comprehensive-only**. Within a module, questions are tagged `[Q]` (asked at every depth) or `[S]` (standard and above) or `[C]` (comprehensive only). Untagged questions count as `[S]`.

The designer may change depth mid-interview ("let's go deeper on combat", "speed this up"). Honor it and record the change in the ledger's notes.

## 3. Answer handling

Every question accepts these meta-answers in addition to a real answer:

| Designer says | Ledger status | What you do |
|---------------|---------------|-------------|
| A real answer | `decided` | Record it verbatim (lightly cleaned), then move on. If the answer is vague, ask one follow-up before accepting it. |
| "You decide" / "pick something sensible" | `assumed` | Propose a specific choice with a one-line rationale tied to the pillars or comparables, state it aloud, record it as `assumed`. The designer can overrule it at any later point. |
| "Skip" / "not relevant" | `skipped` | Record why it does not apply. Do not ask again. |
| "Later" / "I don't know yet" / "need to playtest that" | `open` | Record the question and any partial thinking. Open items are surfaced at the end of the interview, feed the research plan, and appear in the GDD's Open Questions appendix. |
| An answer that changes an earlier decision | `decided` (new) + supersedes old row | Update the earlier row's status to `superseded` with a pointer to the new row. Say aloud what changed. |

Never leave a question with no status. Never silently invent an answer.

## 4. Asking mechanics

- Use the host's structured-input facility (`AskUserQuestion` in Claude Code, `request_user_input` in Codex) whenever a question has a natural closed set of options (platform, business model, player count, weight). Always include an "Other" path through the tool's free-text option. Keep option lists to four or fewer; put the option you would recommend first and label it `(Recommended)` only when you have a real reason.
- Use plain prose for open questions (pitch, fantasy, what a turn looks like). Ask one at a time.
- You may batch up to three closed questions that belong to the same module and do not depend on each other (for example: platform, input method, business model). Never batch open-ended questions.
- Start each module with a one-line header showing progress: `Module 4 of 11 - Difficulty and Onboarding`. Keep it to one line.
- After every two or three modules, reflect back a two- to four-sentence summary of what has been decided so far and ask "anything wrong here?" before continuing. Fix anything they correct.
- If the designer volunteers information that answers a later question, record it now and skip that question when you reach it (say "you already covered this, moving on").
- If the designer gives a long unstructured answer, extract every decision it contains into separate ledger rows.

## 5. The ledger

Maintain `interview-ledger.md` in the session directory. Write it incrementally: after each module (or every ten questions, whichever comes first), so an interruption loses at most one module. The ledger is the single source of truth that the GDD writer and auditor trace against.

```markdown
# Interview Ledger: [Project Name]

Role: [interview-video-game | interview-tabletop]
Depth: [quick | standard | comprehensive]
Started: [ISO timestamp]
Last updated: [ISO timestamp]

## Summary

| Status | Count |
|--------|-------|
| decided | 0 |
| assumed | 0 |
| open | 0 |
| skipped | 0 |
| superseded | 0 |

## Rows

| ID | Module | Question | Answer | Status | Notes |
|----|--------|----------|--------|--------|-------|
| VG-01-01 | 01 Platform and Business | Primary platform? | PC (Steam) first, consoles later | decided | |
| VG-01-02 | 01 Platform and Business | Business model? | Premium, USD 19.99 | assumed | Designer said "you decide"; premium fits the 8-hour narrative scope and comparables (Comparable A, Comparable B) |
| VG-04-03 | 04 Difficulty and Onboarding | Difficulty options? | | open | Designer wants to playtest before committing; leaning toward three presets |
```

Rules:

- IDs are `<prefix>-<module number>-<question number>` where the prefix is `CG` for concept gathering, `VG` for the video game interview, `TT` for the tabletop interview. Numbers are zero-padded to two digits. Never renumber an existing row.
- The `Answer` cell holds the decision in the designer's words, trimmed. Long answers go in the `Notes` cell or a sub-bullet below the table with the row ID as anchor.
- Update the Summary table whenever you write the file. Mirror the counts into `state.json` at `interview.counts`.
- `superseded` rows keep their original answer and add `Superseded by <ID>` in Notes.

## 6. Ending a module

Before leaving a module, run the module's **Exit check** listed in the role file. If any exit item is unmet and the question was not skipped, ask one more targeted question for it. Then append the module's rows to the ledger and add the module to `interview.modulesCompleted` in `state.json`.

## 7. Ending the interview

1. Present the **coverage summary**: modules completed, counts by status, and the full list of `open` and `assumed` items in two short lists.
2. Ask: "Any of these assumptions you want to overrule now, or open questions you can answer?" Process any answers.
3. Ask the closing questions every interview ends with:
   - "What is the one thing this document must get right?"
   - "What are you most worried about with this design?"
   - "Is there anything I did not ask about that matters?"
   Record these as ledger rows in a final module `99 Closing`.
4. Write `details.md` using the role file's output format. `details.md` is the synthesized narrative of the ledger; every claim in it must trace to a ledger row, and it must carry an **Assumptions** section (all `assumed` rows) and an **Open Questions** section (all `open` rows).
5. Finalize the ledger and `state.json` counts.

## 8. Tone

Be a collaborator, not an auditor. Encourage, challenge gently, and stay curious. When an idea is strong, say so specifically. When two answers conflict, point it out without judgment and help resolve it. Keep your own text short; the designer should be doing most of the talking.
