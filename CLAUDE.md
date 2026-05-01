# SCIE3314 APSIM Wiki — Schema for LLM Agents

**Version**: 1.0  
**Date**: May 2026  
**Purpose**: Maintain a persistent, compounding knowledge base for APSIM crop modeling education.

---

## Overview

This is an **LLM Wiki** — a structured, interlinked knowledge base maintained collaboratively by you (curator) and me (agent). Every source ingested refines the wiki. Every question answered adds new synthesis pages.

**Key principle**: The wiki is a persistent artifact, not a chat. Knowledge compounds over time.

---

## Three-Layer Architecture

### Layer 1: Raw Sources (`raw/`)

**Immutable source documents.** LLM reads but never modifies.

- **`sources/`**: APSIM documentation, research papers, best practices, pedagogic frameworks
  - Example: `Holzworth_2014_APSIM.md`, `Zadok_Scale_Reference.md`, `Water_Balance_Equations.md`
  
- **`exercises/`**: Near-solved `.apsimx` files with design notes
  - Example: `wheat_fallow_partial.apsimx`, `wheat_nitrogen_factorial.apsimx`
  - Include design rationale in filename or companion `.md` file
  
- **`instructor_notes/`**: Classroom observations, student misconceptions, timing data, Q&A logs
  - Example: `Common_Student_Questions_Module_2.md`, `Exercise_A_Timing_Data.md`

**Rules**:
- These are your source of truth; never overwritten
- Add new source → trigger ingest workflow (see Operations below)
- One source at a time; stay involved in curation

---

### Layer 2: Wiki (`wiki/`)

**LLM-generated synthesis layer.** Organized by page type (entities, concepts, sources, synthesis, FAQ, exercises).

#### 2a. **Entities** (`wiki/entities/`)

Core APSIM concepts — components, variables, processes.

**Examples**:
- `Wheat.md` — Crop model, parameters, phenology
- `ESW_Extractable_Soil_Water.md` — Definition, role in sowing rules, connection to water balance
- `Manager_Component.md` — Sowing, fertilizer, harvest rules
- `Soil_Layers.md` — Profile structure, water characteristics
- `Thermal_Time.md` — GDD accumulation, cultivar differences, stress effects
- `Zadok_Scale.md` — Phenology stages, frost/heat risk windows
- `Nitrogen_Uptake.md` — Plant demand vs. availability, stress factors

**Format** (template provided below):
- Definition + equation (if applicable)
- Role in APSIM
- Cross-references to related entities
- Relevant equations/diagrams (text-based or placeholder)
- Student misconceptions
- Exercises where this concept is central
- Source citations

---

#### 2b. **Concepts** (`wiki/concepts/`)

Pedagogic frameworks and design principles.

**Examples**:
- `Scaffolding_Strategy.md` — Why near-solved files reduce cognitive load; Bloom's progression
- `Near_Solved_File_Design.md` — How to create `.apsimx` files (what to pre-configure, what students configure)
- `Sowing_Window_Logic.md` — Decision tree for ESW + rainfall thresholds + date windows
- `Water_Stress_Framework.md` — When water limits yield; critical periods
- `Phenology_Thermal_Time_Link.md` — Why thermal time matters for predicting development
- `AEN_Agronomic_Efficiency.md` — N response methodology, calculation, interpretation

**Format**:
- Concept description
- Why it matters pedagogically
- Connection to exercises
- Common misconceptions
- Visual summary (placeholder)

---

#### 2c. **Sources** (`wiki/sources/`)

One summary page per ingested source file.

**Naming**: `[Source_Type]_[Title]_Summary.md`  
**Examples**:
- `Paper_Holzworth_2014_APSIM_Summary.md`
- `Guide_Zadok_Scale_Reference_Summary.md`
- `Instructor_Notes_Common_Questions_Module_2_Summary.md`

**Format**:
```markdown
---
title: "Source: [Title]"
source_type: Paper | Guide | Instructor Notes | Other
date_ingested: 2026-05-01
related_entities: [[Wheat]], [[ESW]], [[Thermal_Time]]
related_modules: 0, 1, 2, 4
---

# [Title]

**Source Type**: Paper / Guide / Instructor Notes  
**Original Date**: Year  
**Key Takeaways**:
- Takeaway 1
- Takeaway 2
- Takeaway 3

**Entities Referenced**: [[Thermal_Time]], [[ESW]], [[Zadok_Scale]], ...  
**Relevant to Modules**: 0, 1, 2, 4  
**Citation**: [Full citation + link to raw/sources/file]

## Summary
[Condensed overview of source content]

## How to Use This Source
- For understanding: [concept]
- For teaching: [how to integrate]
- For exercises: [which exercises benefit]
```

---

#### 2d. **Synthesis** (`wiki/synthesis/`)

Cross-cutting analysis spanning multiple sources and pedagogic intent.

**Examples**:
- `N_Response_Teaching_Path.md` — Full pathway: Exercise B (raw) + Holzworth (source) + AEN (concept) + student Q&A
- `Phenology_Sowing_Connection.md` — How thermal time + phenology inform sowing window decisions
- `Water_Stress_Risk_Framework.md` — Integrate ESW, rainfall, PET, root depth into risk assessment
- `Module_0_7_Dependencies.md` — Learning progression; prerequisite map
- `Exercise_A_to_D_Narrative.md` — How exercises build skill; conceptual arc

**Format**:
- Research question or learning goal
- Multi-source synthesis
- Citations throughout
- Integration with pedagogic strategy
- Diagram placeholders

---

#### 2e. **FAQ** (`wiki/faq/`)

Distilled Q&A from students, instructors, and troubleshooting.

**Examples**:
- `Crop_Won't_Emerge.md` — Decision tree: check sowing rule, ESW, temperature, date window
- `Yield_Unrealistic.md` — Diagnostic checklist: N applied? Water stress? Phenology advancing?
- `Report_Variables_Empty.md` — Variable naming, parent relationships, did simulation run?

**Format**:
- Question
- Root causes (bullet list with checks)
- Solution steps
- Citations to relevant entity/concept pages
- Prevention tips

---

#### 2f. **Exercises** (`wiki/exercises/`)

Detailed guides for each exercise (A–D, plus modules 0–7).

**Naming**: `Exercise_[Letter_or_Module]_[Name]_Guide.md`

**Examples**:
- `Exercise_A_Fallow_Guide.md`
- `Exercise_B_Nitrogen_Guide.md`
- `Module_0_Why_APSIM_Guide.md`
- `Module_4_Nitrogen_Fertilisation_Guide.md`

**Format**:
```markdown
---
title: "Exercise A: Fallow Water Balance"
module: 2
learning_outcomes: LO1, LO2
estimated_time: 30 min
associated_file: wheat_fallow_partial.apsimx
key_concepts: [[ESW]], [[Water_Balance]], [[Soil_Layers]]
---

# Exercise A: Fallow Water Balance

## Learning Outcomes
- Describe daily water balance components
- Interpret ESW time series
- Identify seasonal runoff patterns

## Key Concepts
- [[ESW]]
- [[Water_Balance]]
- [[Soil_Layers]]

## Time Budget
- Setup: 5 min
- Simulation: 10 min
- Analysis & graphing: 10 min
- Q&A: 5 min

## Student Instructions

### Part 1: Configure Report
[Step-by-step instructions]

### Part 2: Run Simulation
[What students do]

### Part 3: Graph and Interpret
[Graphing task]

## Worked Answer
[Instructor guide]

### Expected Results
[Sample output, graphs, numbers]

### Common Misconceptions
- "ESW increases whenever it rains" — ESW is limited by DUL; excess runs off or drains
- "Fallow loses all water to evaporation" — Some seeps deep as drainage

## Assessment
[Formative Q1, Q2, Q3 to check understanding]

## Troubleshooting
[If simulation crashes, if variables missing, etc.]
```

---

### Layer 3: Image Placeholders (`image_placeholders/`)

**Placeholder system for images to be generated later.**

Each placeholder file documents the image specification (legend, size, content).

**File naming**: `[Page_Name]_[Figure_Number]_PLACEHOLDER.md`

**Examples**:
- `Wheat_Component_Tree_Fig_1_PLACEHOLDER.md`
- `Water_Balance_Flows_Fig_2_PLACEHOLDER.md`
- `Phenology_Zadok_Timeline_Fig_3_PLACEHOLDER.md`

**Format**:
```markdown
# Image Placeholder: [Figure Title]

**Location**: wiki/entities/Wheat.md  
**Figure Number**: Fig 1  
**Dimensions**: 1200 × 600 px (or TBD)  
**Format**: PNG (or SVG for diagrams)  

## Legend / Description

**Title**: [Title of image]  
**Subtitle**: [Descriptive subtitle; use this when generating the image]

[Detailed specification of what the image should contain]

## Content Specification

[Technical details about what to render, color scheme, style, labels, etc.]

## Generated By
[Name of person who created the image, when]

## Status
- [ ] Not started
- [ ] In progress
- [ ] Complete
```

**Workflow**:
1. When writing a wiki page that needs an image, create a `_PLACEHOLDER.md` file first
2. Reference the placeholder in the wiki page: `[[See Image: wheat_tree_fig1_PLACEHOLDER.md]]`
3. Later, when generating images, use the placeholder spec as your brief
4. Once generated, update the wiki page with the image link and mark placeholder as "Complete"

---

## Operations

### Operation 1: Ingest a New Source

**Trigger**: You add a file to `raw/sources/`, `raw/exercises/`, or `raw/instructor_notes/`

**Workflow**:
1. **You**: "I added `Zadok_Scale_Reference.md` to `raw/sources/`"
2. **Me**: Read the source
3. **Me**: Tell you key takeaways
4. **Me**: Perform updates:
   - Create `wiki/sources/Guide_Zadok_Scale_Reference_Summary.md`
   - Create or update relevant `wiki/entities/` pages (e.g., [[Zadok_Scale.md]], [[Phenology.md]])
   - Update relevant `wiki/synthesis/` pages if cross-cutting (e.g., [[Phenology_Sowing_Connection.md]])
   - Create image placeholders if diagrams needed
   - Add entry to `wiki/log.md`: `## [2026-05-01] ingest | Guide: Zadok Scale Reference`
5. **You**: Review updates; confirm or redirect

**Tips**:
- Ingest one source at a time; stay involved
- Use wikilinks ([[PageName]]) throughout; I'll create missing pages as needed
- Tell me if a source contradicts existing claims; I'll flag it

---

### Operation 2: Answer a Question

**Trigger**: You ask a question or share student Q&A

**Workflow**:
1. **You**: "Why doesn't thermal time accumulate when it's very hot?"
2. **Me**: Search relevant wiki pages (using index to find starting points)
3. **Me**: Synthesize answer citing wiki pages + sources
4. **Me** (Optional): If answer is valuable, file it as a new FAQ page or synthesis page
5. **Log**: Add entry: `## [2026-05-01] query | "Thermal time in heat stress" → wiki/entities/Thermal_Time.md (updated)`

---

### Operation 3: Lint the Wiki (Monthly or After Large Ingests)

**Trigger**: Periodic health check or after significant updates

**Checks**:
- Broken wikilinks (e.g., [[NonexistentPage]])
- Stale claims (e.g., "Exercise C uses N=0,80,160" — verify in `.apsimx` file)
- Orphan pages (no inbound links; should they exist?)
- Missing concepts (e.g., "thermal time" mentioned 20x; dedicated entity page exists?)
- Contradictions (e.g., two pages say conflicting things about sowing window)
- Incomplete placeholders (image specs but image not generated)

**Workflow**:
1. **Me**: Run lint; report findings
2. **You**: Decide what to fix
3. **Me**: Update pages

---

## Wiki Conventions

### Wikilinks
Use `[[PageName]]` for cross-references. I'll create missing pages as needed.

**Examples**:
- `[[ESW]]` → links to `wiki/entities/ESW_Extractable_Soil_Water.md`
- `[[Scaffolding_Strategy]]` → links to `wiki/concepts/Scaffolding_Strategy.md`
- `[[Holzworth_2014_APSIM_Summary]]` → links to `wiki/sources/Paper_Holzworth_2014_APSIM_Summary.md`

### Frontmatter
Every page includes YAML frontmatter:

```yaml
---
title: Page Title
tags: [module-2, water-balance, soil]
related: [[ESW]], [[Water_Balance]]
last_updated: 2026-05-01
---
```

### Conventions
- **Page names**: CamelCase or Underscored_Case (no spaces)
- **Citations**: Every claim cites a source (e.g., "See [[Holzworth_2014_APSIM_Summary]]")
- **Cross-references**: Use wikilinks liberally
- **Image references**: Use placeholder markdown file references
- **Code/Math**: Inline `` `code` `` or `$$equation$$` blocks

---

## Page Template: Entity

```markdown
---
title: "Entity: [Name]"
tags: [entity, module-X, concept-area]
related: [[RelatedEntity1]], [[RelatedEntity2]]
last_updated: 2026-05-01
---

# [Name]

## Definition
[Clear, concise definition. Include equation if applicable.]

$$
\text{Formula or equation}
$$

## Role in APSIM
[What does this entity do in simulations? How does it interact with other components?]

## Key Parameters
| Parameter | Typical Range | Notes |
|-----------|---------------|-------|
| Param1 | 0–100 | Description |

## Related Concepts
- [[RelatedEntity1]]: How it connects
- [[RelatedEntity2]]: How it connects

## Exercises Where This is Central
- [[Exercise_A_Fallow_Guide]]: How it's used
- [[Exercise_B_Nitrogen_Guide]]: How it's used

## Common Student Misconceptions
- **Misconception 1**: [Description] → Clarification
- **Misconception 2**: [Description] → Clarification

## Troubleshooting
[Common issues + diagnostic checks]

## Sources
- [[Paper_Holzworth_2014_APSIM_Summary]]: Relevant section
- [[APSIM_Internal_Guide.md]]: Chapter X

---

**Last Updated**: 2026-05-01  
**Inbound Links**: [[Synthesis_Page]], [[Exercise_B]]  
**Status**: Complete | Draft | Needs Review
```

---

## Page Template: Concept

```markdown
---
title: "Concept: [Name]"
tags: [concept, pedagogy, module-X]
related: [[RelatedConcept1]], [[Entity1]]
last_updated: 2026-05-01
---

# [Name]

## What Is It?
[Clear explanation of the pedagogic concept]

## Why It Matters
[Why is this important for teaching APSIM? How does it improve learning?]

## Connected to Learning Outcomes
- LO1: How this concept supports it
- LO2: How this concept supports it

## Exercises That Use This Concept
- [[Exercise_A_Fallow_Guide]]: Role
- [[Exercise_B_Nitrogen_Guide]]: Role

## Related Concepts
- [[RelatedConcept1]]: Connection
- [[RelatedConcept2]]: Connection

## Related Entities
- [[Wheat]]: Connection
- [[ESW]]: Connection

## Visual Summary
[[See Image: concept_summary_fig_PLACEHOLDER.md]]

## Sources
- [[Guide_Pedagogic_Framework_Summary]]: Foundation

---

**Last Updated**: 2026-05-01  
**Inbound Links**: [[Synthesis_Module_0_7_Dependencies]]
```

---

## Page Template: FAQ

```markdown
---
title: "FAQ: [Question]"
tags: [faq, troubleshooting, module-X]
related: [[Entity1]], [[Exercise_A]]
last_updated: 2026-05-01
---

# FAQ: [Question]

## The Question
[Restate question clearly]

## Quick Answer
[One-sentence answer]

## Root Causes
[Diagnostic decision tree with checks]

### Check 1: [Condition?]
- If true → see Solution A
- If false → see Check 2

### Check 2: [Condition?]
- If true → see Solution B
- If false → see Check 3

## Solutions

### Solution A: [Title]
[Steps to resolve]

### Solution B: [Title]
[Steps to resolve]

## Prevention
[How to avoid this issue]

## Related Pages
- [[Entity_Involved]]: Background
- [[Exercise_Where_This_Arises]]: Context

## Sources
- [[Source_Summary]]: Relevant section

---

**Last Updated**: 2026-05-01  
**Inbound Links**: [[log.md]], [[Synthesis_Troubleshooting]]
```

---

## Index Page (`wiki/index.md`)

Auto-updated master catalog. Format:

```markdown
# SCIE3314 APSIM Wiki Index

**Last Updated**: 2026-05-01  
**Total Pages**: [N]  
**Total Sources Ingested**: [N]  
**Image Placeholders**: [N] (of which [M] complete)

## Entities (APSIM Components & Concepts)
- [[Wheat]] — Crop model
- [[ESW]] — Extractable soil water
- [[Manager_Component]] — Sowing, fertilizer, harvest rules
- ... (alphabetical or grouped by domain)

## Concepts (Pedagogic)
- [[Scaffolding_Strategy]]
- [[Near_Solved_File_Design]]
- ... (alphabetical)

## Sources (Ingested)
- [[Paper_Holzworth_2014_APSIM_Summary]]
- [[Guide_Zadok_Scale_Reference_Summary]]
- ... (by date ingested)

## Synthesis (Cross-Cutting Analysis)
- [[N_Response_Teaching_Path]]
- [[Phenology_Sowing_Connection]]
- ... (by topic)

## FAQ
- [[Crop_Won't_Emerge]]
- [[Yield_Unrealistic]]
- ... (alphabetical)

## Exercises
### Standalone Exercises (Modules 0–7)
- [[Module_0_Why_APSIM_Guide]]
- [[Module_1_Structure_Guide]]
- ...

### Integrated Exercises (A–D, may span modules)
- [[Exercise_A_Fallow_Guide]]
- [[Exercise_B_Nitrogen_Guide]]
- ...
```

---

## Log File (`wiki/log.md`)

Append-only chronological record. Format:

```markdown
# Wiki Ingest & Query Log

## [2026-05-02] ingest | Reference Docs: APSIM Internal Guide
- Created entity pages: Wheat, ESW, Manager_Component, Soil_Layers, Thermal_Time, Zadok_Scale
- Created concept page: Scaffolding_Strategy
- Updated: wiki/index.md
- Image placeholders created: 5

## [2026-05-02] ingest | Reference Docs: Component Reference
- Updated entity pages: Wheat, Manager_Component, Soil_Layers
- Created source summary: APSIM_Internal_Guide_Summary.md
- Updated: wiki/index.md

## [2026-05-03] query | "Why thermal time at 0°C?"
- Answer: See [[Thermal_Time.md]] (updated with stress section)
- New FAQ: [[Thermal_Time_During_Cold_Snap.md]]
- Sources cited: [[Holzworth_2014_APSIM_Summary]]

## [2026-05-10] lint | Monthly health check
- Found: [[Vernalization]] mentioned but no entity page → created
- Found: Stale claim in [[Exercise_B_Nitrogen_Guide]] (N rates) → updated against `.apsimx` file
- Found: 3 orphan pages → linked or scheduled for deletion
```

---

## Your Role vs. My Role

| Task | You | Me |
|------|-----|-----|
| **Source curation** | Decide what to ingest, in what order | — |
| **Question-asking** | Ask questions, share student Q&A | — |
| **Human judgment** | Review updates, steer direction, resolve contradictions | — |
| **Reading & summarizing** | — | Extract key information from sources |
| **Cross-referencing** | — | Create wikilinks, update related pages |
| **Consistency checking** | — | Lint for broken links, contradictions, stale claims |
| **Page creation/updating** | — | Write entity, concept, FAQ pages |
| **Wikipedia maintenance** | — | Add entries to index, update log |

---

## Tips & Tricks

- **Start small**: Begin with entities/ (core APSIM concepts), then expand
- **Iterate**: First pages are rough; they refine as you add sources
- **No hallucinations**: I cite sources or flag if something isn't in raw/
- **Git for free**: Commit after each ingest; you have version history
- **Obsidian optional**: If you want a prettier UI + graph visualization, clone wiki repo locally and open in Obsidian
- **Search**: As wiki grows, maintain the index.md; it's your map

---

## Next Steps

1. Ingest existing reference docs (APSIM_Internal_Guide, Component_Reference, Troubleshooting) → initial entity/concept pages
2. Add APSIM papers, pedagogic resources → enrich synthesis
3. Collect student Q&A from teaching → update FAQ
4. Monthly lint → keep wiki healthy

---

**Last Updated**: May 2026  
**Version**: 1.0  
**Maintained By**: SCIE3314 Team + LLM Agent
