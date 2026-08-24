---
name: status
description: Show the current pipeline status of a GDD project session (completed, in-progress, and pending stages, checkpoint decisions, generated files). Use when the user asks where a Game Design Document project stands or wants to list existing gdd sessions. Read-only; does not continue the pipeline.
---

# gdd:status - report a GDD session's progress

Treat the text supplied with the skill invocation as the project name. If none was supplied, list every directory under `.gdd/sessions/` in the current working directory with its `currentStage` and `updatedAt`, then stop. If `.gdd/sessions/` does not exist, say so and suggest `/gdd:create`.

## Steps

1. Read `.gdd/sessions/<project>/state.json`. If it is missing or unparseable, report that and stop.
2. List the files actually present in the session directory (including `research/` and `supplements/`).
3. Print the report below. Mark a stage `completed` if it is in `completedStages`, `skipped` if a checkpoint decision bypassed it (for example research stages when `researchDecision.decision` is `skip`), `in_progress` if it equals `currentStage`, otherwise `pending`. Flag any completed stage whose expected output file is missing on disk.

```
Pipeline Status: <project>
----------------------
[x] Concept Gathering     - completed
[x] Deep Dive Questions   - completed
[-] Research              - skipped (user chose no research)
[ ] GDD Writing           - in_progress
[ ] GDD Review            - pending
[ ] Supplement Analysis   - pending
[ ] Supplement Selection  - pending
[ ] Supplement Generation - pending
[ ] Index Generation      - pending

Current Stage: GDD_WRITING
Last Updated: <updatedAt>
Checkpoint decisions: research=skip, gddReview=-, supplements=-
Files on disk: concept.md, details.md
Errors: none
```

4. End with the one command that moves the project forward: `/gdd:resume <project>` if not completed, or a note that the session is complete and `INDEX.md` is the entry point.

This skill never modifies files.
