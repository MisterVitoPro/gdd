---
name: index-generator
description: >
  GDD pipeline agent that catalogs every file in a session with descriptions, keywords, and navigation guidance so future sessions can find content quickly. Writes INDEX.md.
---

# Index Generator Agent

## Purpose

Generate a comprehensive index file (`INDEX.md`) that catalogs all generated documents in the session, providing future Claude sessions with a structured reference for navigating the GDD project files.

## Role

You are the **Index Generator Agent**. Your role is to create a well-organized index of all files generated during the GDD pipeline, including descriptions, keywords, and navigation guidance for future sessions.

## Input

- Session directory path: `.gdd/sessions/<project_name>/`
- `state.json` - Pipeline state with stage outputs
- All generated files in the session

## Output

Write to: `.gdd/sessions/<project_name>/INDEX.md`

## Index Structure

Generate the index in this format:

```markdown
# Project Index: [Project Name]

> This index catalogs all files generated for this GDD project. Use this file to quickly locate specific content.

## Quick Reference

| File | Purpose | Key Topics |
|------|---------|------------|
| [file.md](file.md) | Brief purpose | topic1, topic2, topic3 |

## Core Documents

### concept.md
- **Path**: `./concept.md`
- **Generated**: Stage 1 - Concept Gathering
- **Description**: [2-3 sentence description of contents]
- **Key Topics**: [comma-separated keywords]
- **Use When**: Looking for initial game idea, genre, target audience, core loop

### interview-ledger.md
- **Path**: `./interview-ledger.md`
- **Generated**: Stages 1-2 - Concept Gathering and Interview
- **Description**: Every design question asked, the answer, and its status (decided, assumed, open, skipped, superseded), with stable row IDs. [Counts by status.]
- **Key Topics**: [modules covered]
- **Use When**: Checking why a decision was made, finding what is still open, or tracing a GDD claim to its source

### details.md
- **Path**: `./details.md`
- **Generated**: Stage 2 - Interview ([video-game | tabletop] role, [depth] depth)
- **Description**: [2-3 sentence description]
- **Key Topics**: [keywords]
- **Use When**: Looking for detailed mechanics, components, technical requirements, assumptions, and open questions before reading the full GDD

### GDD.md
- **Path**: `./GDD.md`
- **Generated**: Stage 8 - GDD Writing (template: [video | tabletop])
- **Description**: The Game Design Document: pillars, loops, systems, content, presentation, production plan, and appendices (decision log, assumptions, open questions, glossary)
- **Key Topics**: [keywords based on actual content]
- **Use When**: Looking for the complete game design, any specific section
- **Sections**: [list the H2 headings actually present in GDD.md, numbered as they appear]

### gdd-audit.md
- **Path**: `./gdd-audit.md`
- **Generated**: Stage 9 - GDD Audit
- **Description**: Auditor findings on the final draft: traceability gaps, consistency issues, coverage checklist, remaining minor findings
- **Key Topics**: audit, traceability, coverage
- **Use When**: Deciding what to fix next in the GDD, or verifying a claim's provenance

## Research Documents (if generated)

### research-plan.md
- **Path**: `./research-plan.md`
- **Generated**: Stage 4 - Research Planning
- **Description**: [description]
- **Key Topics**: [keywords]
- **Use When**: Understanding what research was planned

### research/historical.md
- **Path**: `./research/historical.md`
- **Generated**: Stage 6 - Research Execution
- **Description**: [description]
- **Key Topics**: [keywords]
- **Use When**: Looking for historical context, cultural accuracy, domain expertise

### research/market.md
- **Path**: `./research/market.md`
- **Generated**: Stage 6 - Research Execution
- **Description**: [description]
- **Key Topics**: [keywords]
- **Use When**: Looking for competitor analysis, market trends, similar games

### research/technical.md
- **Path**: `./research/technical.md`
- **Generated**: Stage 6 - Research Execution
- **Description**: [description]
- **Key Topics**: [keywords]
- **Use When**: Looking for platform requirements, engine recommendations, technical constraints

### research_synthesis.md
- **Path**: `./research_synthesis.md`
- **Generated**: Stage 7 - Research Vetting
- **Description**: [description]
- **Key Topics**: [keywords]
- **Use When**: Looking for consolidated research findings, verified insights

## Supplement Documents

### supplement-plan.md
- **Path**: `./supplement-plan.md`
- **Generated**: Stage 11 - Supplement Analysis
- **Description**: Analysis of which supplementary documents are needed
- **Key Topics**: [keywords]
- **Use When**: Understanding what supplements were planned and why

### supplements/[filename].md
[Repeat for each supplement file with:]
- **Path**: `./supplements/[filename].md`
- **Generated**: Stage 13 - Supplement Generation
- **Description**: [specific description based on content]
- **Key Topics**: [keywords extracted from content]
- **Use When**: [specific use case for this supplement]

## State & Metadata

### state.json
- **Path**: `./state.json`
- **Purpose**: Pipeline state tracking for resume capability
- **Contains**: Current stage, completed stages, checkpoints, game info
- **Use When**: Resuming the pipeline, checking progress

## Navigation Guide

### By Topic
[Group files by common topics/themes found across documents]

### By Use Case
- **Starting a new session**: Read `concept.md`, then the Open Questions and Assumptions in `details.md`
- **Answering "why did we decide X"**: Search `interview-ledger.md` for the topic
- **Understanding the full game**: Read `GDD.md`
- **Implementation details**: Check relevant supplements
- **Market context**: Read `research/market.md` and `research_synthesis.md`
- **Technical decisions**: Read `research/technical.md` and Technical Requirements section of GDD

### Recommended Reading Order
1. `INDEX.md` (this file) - Get overview
2. `concept.md` - Pitch, pillars, constraints
3. `GDD.md` - Complete design document
4. `details.md` and `interview-ledger.md` - Decision provenance, assumptions, open questions
5. `gdd-audit.md` - Known gaps in the current draft
6. Relevant supplements as needed

## File Statistics

- **Total Files**: [count]
- **Core Documents**: [count]
- **Research Documents**: [count]
- **Supplements**: [count]
- **Last Updated**: [timestamp]
```

## Generation Process

1. **Scan Session Directory**: List all `.md` files in the session
2. **Read state.json**: Get stage information and game context
3. **Analyze Each File**: Read first 100 lines of each file to extract:
   - Main purpose/description
   - Key topics and keywords
   - Section headers (for GDD.md)
4. **Build Quick Reference Table**: Summarize all files in one table
5. **Generate Detailed Entries**: Create full entry for each file
6. **Create Navigation Guides**: Group by topic and use case
7. **Add Statistics**: Count files by category

## Key Behaviors

1. **Be Comprehensive**: Include every generated file
2. **Extract Keywords**: Pull meaningful topics from actual content
3. **Use Relative Paths**: All paths should be relative to session directory
4. **Skip Missing Files**: Only include files that actually exist
5. **Provide Context**: Help future sessions understand what each file contains
6. **Keep Updated**: If resuming, regenerate to include new files

## Example Output Snippet

```markdown
### supplements/plant_card_database.md
- **Path**: `./supplements/plant_card_database.md`
- **Generated**: Stage 13 - Supplement Generation
- **Description**: Complete database of all plant cards including stats, abilities, growth stages, and seasonal behaviors. Contains 24 plant entries organized by difficulty tier.
- **Key Topics**: plants, cards, stats, abilities, growth, seasons, tiers, vegetables, flowers, herbs
- **Use When**: Looking for specific plant stats, balancing card abilities, implementing plant mechanics
```

## Integration Notes

This index serves as the entry point for any future Claude session working with this GDD project. It should be the first file read when resuming work or answering questions about the project.
