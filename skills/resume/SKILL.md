---
name: resume
description: Resume an interrupted or paused GDD pipeline session from its last completed stage or checkpoint. Use when the user wants to continue, pick up, or revise a Game Design Document project started with gdd:create.
---

# gdd:resume - continue a GDD session

Treat the text supplied with the skill invocation as the project name. If none was supplied, list the directories under `.gdd/sessions/` in the current working directory with each one's `currentStage` and `updatedAt`, and ask which to resume. If `.gdd/sessions/` does not exist, say so and suggest `/gdd:create`.

## Steps

1. Read `.gdd/sessions/<project>/state.json`. If it is missing or unparseable, report that and stop; do not guess.
2. **Migrate old sessions.** If `version` is `1.0` (created before the forked interview), upgrade in place before doing anything else:
   - Rename stage `DEEP_DIVE` to `INTERVIEW` in `currentStage` and `completedStages`. Rename `stageOutputs.details` as-is; add the missing keys from `../../templates/state_template.json` (`interviewLedger`, `auditReport`, `interview`, `gameInfo.family`, `gameInfo.pillars`, `checkpoints.interviewDepth`) with their default values.
   - Set `gameInfo.family` to `video` if `gameInfo.type` is a video game, otherwise `tabletop`.
   - If the session had completed `DEEP_DIVE` but has no `interview-ledger.md`, warn that the GDD audit will have no ledger to trace against and offer to run the new interview (`INTERVIEW` stage, `Restart the current stage`) before writing.
   - Set `version` to `2.0` and save. Tell the user what was migrated.
3. Print the progress table in the format used by `../status/SKILL.md` (completed, skipped, in progress, pending stages plus interview coverage, current stage, and last update).
4. Check for partial work at the current stage:
   - `INTERVIEW`: compare `interview.modulesCompleted` against `interview.modulesPlanned` and report which modules remain. The interview resumes at the first unfinished module, reusing every row already in `interview-ledger.md`.
   - `RESEARCH_EXECUTION`, `SUPPLEMENT_GENERATION`, `HTML_GENERATION`: compare the expected outputs against the files actually present and report which are missing.
5. Ask the user, with the host's structured-input facility when available:
   1. Continue from the current stage
   2. Restart the current stage (re-run it and overwrite its outputs)
   3. Go back to the previous checkpoint (`RESEARCH_DECISION`, `RESEARCH_APPROVAL`, `GDD_REVIEW`, or `SUPPLEMENT_SELECTION`, whichever most recently preceded the current stage)
6. Update `state.json` (`currentStage`, `updatedAt`) to reflect the choice, then read `../create/SKILL.md` and follow its pipeline from that stage forward. Reuse every existing output file; only regenerate what the user chose to restart. Use the same role-loading rules, dispatch template, checkpoints, and error handling as `gdd:create`.

If `currentStage` is already `COMPLETED`, offer instead to:

- **Resolve open questions**: read the `open` rows from `interview-ledger.md`, ask them one at a time using the interview protocol, update the ledger and `details.md`, then re-run `GDD_WRITING` in revision mode limited to the affected sections, followed by `GDD_AUDIT` and `GDD_REVIEW`.
- **Revise GDD.md**: re-run `GDD_REVIEW` with revision feedback.
- **Extend the interview**: run one or more modules that were skipped at the chosen depth (list them from `modulesPlanned` versus the role file's full module list), then revise the GDD as above.
- **Generate additional supplements**: re-run `SUPPLEMENT_SELECTION` with the remaining items from `supplement-plan.md`.
- **Rebuild INDEX.md**.
- **Generate or refresh the HTML reading edition**: follow `../html/SKILL.md`. Safe to run repeatedly; it overwrites `html/` and does not alter `currentStage` on a `COMPLETED` session.

After any of these, re-run `INDEX_GENERATION` so the index stays accurate, and append a row to the GDD's Document History table describing the revision. If `html/` exists, regenerate it too - stale HTML that disagrees with the Markdown is worse than no HTML.
