---
name: status
description: Show the current pipeline status of a GDD project session (completed, in-progress, and pending stages, interview coverage, checkpoint decisions, generated files). Use when the user asks where a Game Design Document project stands or wants to list existing gdd sessions. Read-only; does not continue the pipeline.
---

# gdd:status - report a GDD session's progress

Treat the text supplied with the skill invocation as the project name. If none was supplied, list every directory under `.gdd/sessions/` in the current working directory with its `currentStage`, `gameInfo.family`, and `updatedAt`, then stop. If `.gdd/sessions/` does not exist, say so and suggest `/gdd:create`.

## Steps

1. Read `.gdd/sessions/<project>/state.json`. If it is missing or unparseable, report that and stop. If `version` is `1.0`, note that the session predates the forked interview and that `/gdd:resume` will migrate it.
2. List the files actually present in the session directory (including `research/` and `supplements/`).
3. Print the report below. Mark a stage `completed` if it is in `completedStages`, `skipped` if a checkpoint decision bypassed it (for example research stages when `researchDecision.decision` is `skip`, or supplement generation when no supplements were selected), `in_progress` if it equals `currentStage`, otherwise `pending`. Flag any completed stage whose expected output file is missing on disk.

```
Pipeline Status: <project>
----------------------
Game: <gameInfo.family> / <gameInfo.type> - <gameInfo.genre>

[x] Concept Gathering     - completed
[x] Interview             - completed (video-game, standard depth, 11/11 modules)
[-] Research              - skipped (user chose no research)
[x] GDD Writing           - completed
[ ] GDD Audit             - in_progress
[ ] GDD Review            - pending
[ ] Supplement Analysis   - pending
[ ] Supplement Selection  - pending
[ ] Supplement Generation - pending
[ ] Index Generation      - pending

Interview ledger: 48 decided, 9 assumed, 4 open, 2 skipped
Current Stage: GDD_AUDIT
Last Updated: <updatedAt>
Checkpoint decisions: depth=standard, research=skip, gddReview=-, supplements=-
Files on disk: concept.md, interview-ledger.md, details.md, GDD.md
Errors: none
```

For the Interview line, show `interview.role`, `interview.depth`, and `modulesCompleted.length/modulesPlanned.length` from `state.json`. The ledger counts come from `interview.counts`; if they are all zero but `interview-ledger.md` exists, count the rows by their Status column instead.

4. If the ledger has open items, list them briefly under the report (they are the design decisions still owed) so the user knows what `/gdd:resume` will raise.
5. End with the one command that moves the project forward: `/gdd:resume <project>` if not completed, or a note that the session is complete and `INDEX.md` is the entry point.

This skill never modifies files.
