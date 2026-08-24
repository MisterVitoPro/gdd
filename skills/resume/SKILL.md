---
name: resume
description: Resume an interrupted or paused GDD pipeline session from its last completed stage or checkpoint. Use when the user wants to continue, pick up, or revise a Game Design Document project started with gdd:create.
---

# gdd:resume - continue a GDD session

Treat the text supplied with the skill invocation as the project name. If none was supplied, list the directories under `.gdd/sessions/` in the current working directory with each one's `currentStage` and `updatedAt`, and ask which to resume. If `.gdd/sessions/` does not exist, say so and suggest `/gdd:create`.

## Steps

1. Read `.gdd/sessions/<project>/state.json`. If it is missing or unparseable, report that and stop; do not guess.
2. Print the progress table in the format used by `../status/SKILL.md` (completed, skipped, in progress, pending stages plus the current stage and last update).
3. Check for partial work at the current stage: for parallel stages (`RESEARCH_EXECUTION`, `SUPPLEMENT_GENERATION`) compare the expected outputs against the files actually present and report which are missing.
4. Ask the user, with the host's structured-input facility when available:
   1. Continue from the current stage
   2. Restart the current stage (re-run it and overwrite its outputs)
   3. Go back to the previous checkpoint (`RESEARCH_DECISION`, `RESEARCH_APPROVAL`, `GDD_REVIEW`, or `SUPPLEMENT_SELECTION`, whichever most recently preceded the current stage)
5. Update `state.json` (`currentStage`, `updatedAt`) to reflect the choice, then read `../create/SKILL.md` and follow its pipeline from that stage forward. Reuse every existing output file; only regenerate what the user chose to restart. Use the same role-loading rules, dispatch template, checkpoints, and error handling as `gdd:create`.

If `currentStage` is already `COMPLETED`, offer instead to: revise `GDD.md` (re-run GDD_REVIEW with revision feedback), generate additional supplements (re-run SUPPLEMENT_SELECTION with the remaining items from `supplement-plan.md`), or rebuild `INDEX.md`. After any of these, re-run INDEX_GENERATION so the index stays accurate.
