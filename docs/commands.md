# GDD Pipeline Commands Reference

## Overview

These commands orchestrate the Game Doc Forge pipeline for creating comprehensive Game Design Documents.

---

## Command: `/gdd:create`

### Full Pipeline Execution

**Usage:**
```
/gdd:create [project_name]
```

**Arguments:**
- `project_name` (optional): Name for this GDD project. If not provided, will prompt for one.

**Description:**
Runs the complete GDD generation pipeline from initial concept to supplementary documents.

**Process:**
1. **Concept Gathering**: Interactive Q&A about game fundamentals
2. **Deep Dive**: Follow-up questions based on game type
3. **Research Decision Checkpoint**: User chooses to research or skip
4. **Research Planning**: Identify needed research topics (if enabled)
5. **Research Approval Checkpoint**: User selects topics (if researching)
6. **Research Execution**: Parallel web search research (if enabled)
7. **Research Vetting**: Synthesize and verify findings (if researched)
8. **GDD Writing**: Generate comprehensive document
9. **GDD Review Checkpoint**: User reviews document
10. **Supplement Analysis**: Identify needed supplements
11. **Supplement Selection Checkpoint**: User selects supplements
12. **Supplement Generation**: Create selected documents

**Output:**
```
.gdd/sessions/<project_name>/
├── state.json
├── concept.md
├── details.md
├── research/ (optional)
├── research_synthesis.md (optional)
├── GDD.md
└── supplements/
```

**Example:**
```
/gdd:create stellar-conquest
/gdd:create "My Card Game"
/gdd:create
```

---

## Command: `/gdd:resume`

### Resume Existing Session

**Usage:**
```
/gdd:resume [project_name]
```

**Arguments:**
- `project_name` (required): Name of existing GDD project to resume

**Description:**
Resume a pipeline from its last checkpoint or completed stage.

**Process:**
1. Read `state.json` to determine current stage
2. Display progress summary
3. Continue from last completed stage
4. Preserve all previous outputs

**Example:**
```
/gdd:resume stellar-conquest
```

---

## Command: `/gdd:status`

### Check Pipeline Status

**Usage:**
```
/gdd:status [project_name]
```

**Arguments:**
- `project_name` (required): Name of GDD project to check

**Description:**
Display current pipeline status and completed stages.

**Output Example:**
```
Pipeline Status: stellar-conquest
----------------------
[x] Concept Gathering    - completed
[x] Deep Dive Questions  - completed
[x] Research Decision    - skipped (user chose no research)
[ ] GDD Writing          - in_progress
[ ] GDD Review           - pending
[ ] Supplement Analysis  - pending
[ ] Supplement Selection - pending
[ ] Supplement Generation - pending

Current Stage: GDD Writing
Last Updated: 2024-01-15 14:30:00
```

---

## Checkpoint Interactions

### Research Decision Checkpoint

After deep dive questioning, the pipeline presents:

```
Your game concept and details have been captured.

Would you like to conduct research before writing the GDD?

Research is recommended if your game involves:
- Historical periods or real-world settings
- Technical platforms with specific requirements
- Competitive markets where you want to understand similar games

Options:
1. Conduct research (recommended for games with real-world elements)
2. Skip research (proceed directly to GDD writing)
```

### Research Approval Checkpoint

If research is enabled, shows identified topics:

```
Research Topics Identified:
---------------------------
Historical/Domain Research:
  1. [HIGH] Medieval siege warfare tactics
  2. [MEDIUM] Castle architecture and defense

Market Research:
  1. [HIGH] Tower defense games 2023-2024
  2. [MEDIUM] Mobile strategy game monetization

Technical Research:
  1. [HIGH] Unity mobile optimization
  2. [LOW] Cross-platform save systems

Select topics to research:
- Enter numbers (e.g., "1,2,5")
- Enter "all" for all topics
- Enter "high" for high priority only
- Enter "skip" to skip research
```

### GDD Review Checkpoint

After GDD generation:

```
GDD has been generated: .gdd/sessions/<project>/GDD.md

Document Overview:
- 15 sections completed
- ~X,XXX words
- Key sections: [list of major sections]

Options:
1. Approve and continue to supplement analysis
2. Request revisions (specify sections to revise)
3. Export current state and pause
```

### Supplement Selection Checkpoint

After supplement analysis:

```
Recommended Supplements:
------------------------
[HIGH PRIORITY]
  1. Unit Stats Table - Essential for balance implementation
  2. Upgrade Tree Documentation - Complex progression system
  3. Level Progression Chart - Core gameplay loop

[MEDIUM PRIORITY]
  4. Resource Balance Sheet - Economy tuning
  5. Achievement List - Player motivation

[LOW PRIORITY]
  6. Lore Bible - World building depth
  7. Sound Effect Catalog - Audio reference

Select supplements to generate:
- Enter numbers (e.g., "1,2,3")
- Enter "recommended" for HIGH priority
- Enter "all" for all supplements
- Enter "none" to complete without supplements
```

---

## State Management

### State File Structure

Pipeline state is tracked in `.gdd/sessions/<project>/state.json`:

```json
{
  "version": "1.0",
  "projectName": "project-name",
  "createdAt": "ISO timestamp",
  "updatedAt": "ISO timestamp",
  "currentStage": "STAGE_NAME",
  "completedStages": ["STAGE_1", "STAGE_2"],
  "stageOutputs": {
    "concept": "concept.md",
    "details": "details.md"
  },
  "checkpoints": {
    "researchDecision": {
      "reached": true,
      "decision": "skip"
    }
  },
  "config": {
    "enableWebSearch": true
  }
}
```

### Stage Names

- `NOT_STARTED`
- `CONCEPT_GATHERING`
- `DEEP_DIVE`
- `RESEARCH_DECISION`
- `RESEARCH_PLANNING`
- `RESEARCH_APPROVAL`
- `RESEARCH_EXECUTION`
- `RESEARCH_VETTING`
- `GDD_WRITING`
- `GDD_REVIEW`
- `SUPPLEMENT_ANALYSIS`
- `SUPPLEMENT_SELECTION`
- `SUPPLEMENT_GENERATION`
- `COMPLETED`

---

## Error Handling

If the pipeline encounters an error:

1. State is saved to allow resume
2. Error is logged to `state.json`
3. User is notified with options:
   - Retry the failed stage
   - Skip to next stage (if possible)
   - Abort and save progress

---

## Output Files

### concept.md
Initial game concept including type, genre, theme, audience, and core loop.

### details.md
Detailed game design information from deep dive questioning.

### research-plan.md
Identified research topics with priorities and questions.

### research/historical.md
Historical and domain research findings.

### research/market.md
Market analysis and competitor research.

### research/technical.md
Technical requirements and platform research.

### research_synthesis.md
Consolidated and verified research insights.

### GDD.md
Complete Game Design Document with all 15 sections.

### supplement-plan.md
Analysis of recommended supplementary documents.

### supplements/*.md
Individual supplementary documents as selected.
