# SCIE3314 APSIM Wiki — Ingest & Query Log

**Purpose**: Chronological append-only record of all ingests, queries, and maintenance actions.

---

## [2026-05-01] bootstrap | Wiki structure initialized

**Type**: Bootstrap  
**Details**: 
- Created three-layer architecture: raw/ + wiki/ + image_placeholders/
- Subfolder structure: entities/, concepts/, sources/, synthesis/, faq/, exercises/
- Created CLAUDE.md schema (1.0)
- Created README.md project overview
- Created index.md and log.md (this file)

**Pages Created**: 
- CLAUDE.md (schema)
- README.md (project overview)
- wiki/index.md (master catalog)
- .gitignore (git configuration)

**Status**: Ready for first source ingest

---

## [2026-05-01] ingest | APSIM_Internal_Guide

**Type**: Ingest (first source)  
**Source**: `REFERENCE_DOCS/APSIM_Internal_Guide.md` (3000+ lines)  
**Processing Time**: ~30 min  

**Entities Created**: 11 new pages
- [[Clock]] — Simulation timing and sequencing
- [[Weather]] — Climate input data and PET
- [[Soil]] — Soil profile and state tracking
- [[Soil_Layers]] — Multi-layer structure and properties
- [[Manager]] — Sowing, fertilizer, harvest rules
- [[Report]] — Output variable specification
- [[DataStore]] — Simulation results database
- [[Nitrogen_Uptake]] — Plant N demand and soil supply
- [[Zadok_Scale]] — Phenological stage classification

**Entities Updated**: 4 bootstrap pages enriched
- [[Wheat]] — Added cross-references to Manager, Report, Nitrogen_Uptake
- [[Thermal_Time]] — Linked to Zadok_Scale, Weather
- [[Water_Balance]] — Linked to Soil, ESW, Weather
- [[ESW_Extractable_Soil_Water]] — Linked to Manager sowing rules, Soil

**Source Summary Created**: 
- [[Guide_APSIM_Internal_Summary]] — 400-line reference page

**Index Updated**:
- Entities section reorganized (3 categories: Control/Input, Soil/Water, Crop, Output)
- Sources section populated with APSIM guide summary
- Metadata: 1 source ingested, 13 entities created/updated

**Wikilinks**: ~80 cross-references established (entities → related concepts/exercises)

**Status**: ✅ Ingest complete; ready for next source or concept page creation

---

## [2026-05-01] ingest | wheat_example.apsimx Structure Analysis

**Type**: Ingest (second source; technical reference)  
**Source**: `Apsim_examples/wheat_example.apsimx` (1100 lines JSON)  
**Processing Time**: ~20 min  

**Approach**:
- Read file in 200-line chunks to extract JSON structure
- Document component hierarchy (Clock, Weather, Soil, Manager, Report, DataStore)
- Extract Manager rules (3 parametrized rules: sow, fertilise, harvest)
- Analyze soil profile (7-layer Vertosol, Norwin QLD, PAW=361mm)
- Extract Report configuration (13 daily output variables)
- Synthesize into design patterns for near-solved exercise files

**Source Summary Created**:
- [[wheat_example_Structure_Analysis]] — 700+ line technical reference
  - File structure hierarchy with complete JSON breakdown
  - Clock, Weather, Soil profile (7 layers with parameters)
  - Manager rules with parameter explanations
  - Report variables (LAI, Zadok stage, biomass, grain metrics, yield)
  - Near-solved design principles (pre-configured vs. student-modifiable)
  - Modification workflows for Exercises A–D (fallow, N response, sowing date, 20-year)

**Index Updated**:
- Sources section: Added wheat_example summary
- Metadata: 2 sources ingested

**Wikilinks**: Linked to [[Manager]], [[Soil]], [[Report]], [[DataStore]], [[Clock]], [[Weather]], [[ESW_Extractable_Soil_Water]], [[Nitrogen_Uptake]], [[Zadok_Scale]]

**Pedagogic Outcome**: 
- Tutorial developers now have concrete template for creating Exercise files
- Know exactly how to parametrize Manager rules
- Understand Report variable selection strategy
- Can create near-solved variants (A–D) by cloning + modifying parameters

**Status**: ✅ Ingest complete; wheat_example pattern now documented; ready for Exercise A–D file creation

---

## Ingest Workflow Log

### Format: `## [YYYY-MM-DD] [type] | [source_title]`

**Types**:
- `ingest` — Source added to raw/; wiki updated
- `query` — Question answered; potentially new FAQ/synthesis created
- `lint` — Health check performed; issues flagged/fixed
- `update` — Wiki page updated (external fix or clarification)

---

## Query Workflow Log

*[Queries and answers will be logged here as they occur]*

---

## Maintenance Log

### Monthly Lint Checks

*[To be scheduled monthly]*

---

## Notes

- One ingest/query per line for readability
- Complex ingests may span multiple lines or reference detail files
- Use grep for quick searches: `grep "^## \[" log.md | tail -10` (last 10 entries)

---

**Next Action**: Add first source to raw/sources/; trigger ingest workflow
