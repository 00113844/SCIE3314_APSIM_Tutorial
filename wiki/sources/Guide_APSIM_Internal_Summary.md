---
title: Guide_APSIM_Internal_Summary
tags: [source, architecture, reference]
related: [[APSIM_Internal_Guide]]
status: draft
---

# Source: APSIM Internal Reference Guide — Summary

**Source Document**: `REFERENCE_DOCS/APSIM_Internal_Guide.md` (3000+ lines)  
**Ingest Date**: May 1, 2026  
**Status**: Draft (integrated into 11 entity pages)

---

## Purpose

The APSIM Internal Reference Guide provides detailed explanation of APSIM architecture, component interactions, common pitfalls, and troubleshooting for SCIE3314 instructors and modelers. This source summary references the key entities extracted.

---

## Extracted Entities

### Core Components (9 pages)

| Entity | Focus | Key Concepts |
|--------|-------|---|
| **[[Clock]]** | Simulation time control | Timesteps, date ranges, synchronization |
| **[[Weather]]** | Daily climate input | PET, AET, regional patterns, file format |
| **[[Soil]]** | Soil profile + state | Layer structure, water/N pools, transformations |
| **[[Soil_Layers]]** | Multi-layer structure | Texture variation, drainage rates, root distribution |
| **[[Manager]]** | Simulation control logic | Sowing rules, fertilizer timing, harvest decisions |
| **[[Report]]** | Output variable selection | DataStore integration, variable paths, filtering |
| **[[DataStore]]** | Output database | Export formats (Excel, CSV), multi-simulation handling |
| **[[Wheat]]** | Crop model | Phenology, biomass, grain yield, stress responses |
| **[[Nitrogen_Uptake]]** | Plant N demand | Uptake calculation, stage-dependent demand, AEN |

### Physiological Processes (4 pages)

| Entity | Focus | Key Concepts |
|--------|-------|---|
| **[[Thermal_Time]]** | GDD accumulation | Temperature effects, phenological development, regional variation |
| **[[Water_Balance]]** | Daily water accounting | ESW update, runoff, drainage, evaporation, regional patterns |
| **[[Zadok_Scale]]** | Phenological stages (0–99) | Stage-specific management, GDD mapping, harvest criteria |
| **[[ESW_Extractable_Soil_Water]]** | Plant-available water | Sowing thresholds, water stress, daily dynamics |

---

## Key Architecture Insights

### Daily Simulation Sequence

```
Clock → Weather → Manager → Soil → Crop → Report → DataStore
```

**Critical**: Sequencing ensures causality (weather drives soil, soil constrains crop, Manager decisions trigger actions).

### Three-Layer Problem Representation

1. **Raw**: APSIM component XML + manager rules (immutable specification)
2. **Wiki**: Entity pages explain each component + relationships
3. **Query**: Answer "why" questions by synthesizing entities (e.g., "Why don't roots access deep water?" → See [[Soil_Layers]] + [[Water_Balance]])

### Pedagogic Sequencing (0–7 Modules)

- **Module 0**: Why APSIM? (Need for decision support)
- **Module 1**: Structure (Components, interactions, [[Clock]] + [[Weather]] + [[Soil]])
- **Module 2**: First simulation (Fallow; [[Water_Balance]] only)
- **Module 3**: Reading output ([[Report]], [[DataStore]], interpretation)
- **Module 4**: Output analysis (Plots, seasonal patterns)
- **Module 5**: N fertilisation ([[Manager]] rules, [[Nitrogen_Uptake]], [[AEN_Efficiency]])
- **Module 6**: Phenology ([[Thermal_Time]], [[Zadok_Scale]], stage-dependent N/water stress)
- **Module 7**: Long-term scenarios (Multi-year [[Clock]], rainfall variability)

---

## Common Pitfalls (From Guide)

### Immediate Crashes

1. **Clock date mismatch** → Weather file ends before Clock; simulation crashes Day 50
2. **Weather file not found** → Relative/absolute path error
3. **Soil profile incomplete** → Missing DUL/LL/KS on some layers
4. **Manager syntax error** → JavaScript typos in rule

### Silent Failures (Simulation runs but wrong results)

1. **Crop never sows** → ESW threshold never met; or sowing window too short
2. **Yield unrealistic** → Profile too shallow; underestimate available water
3. **N response zero** → Crop already N-sufficient; or water-limited (not N-limited)
4. **Output empty** → Variable path incorrect (e.g., `ESW` instead of `[Soil].SoilWater.ESW`)

---

## Integrated Workflows

### Exercise A (Fallow 7-Year)

**Entities involved**: [[Clock]], [[Weather]], [[Soil]], [[Water_Balance]], [[Report]], [[DataStore]]  
**Learning**: How rainfall, evaporation, and drainage interact; no [[Manager]] (no crop); tests water-only balance.

### Exercise B (N Response 4-Treatment)

**Entities involved**: [[Clock]], [[Weather]], [[Manager]], [[Soil]], [[Nitrogen_Uptake]], [[Wheat]], [[Report]], [[DataStore]]  
**Learning**: How [[Manager]] rules apply different N rates; [[Nitrogen_Uptake]] response; calculation of [[AEN_Efficiency]].

### Exercise C (Sowing Date 3-Scenario)

**Entities involved**: [[Clock]], [[Manager]] (vary sowing date), [[Thermal_Time]], [[Zadok_Scale]], [[Wheat]]  
**Learning**: Same [[Thermal_Time]] GDD accumulation but different calendar dates; trade-offs (early = long season, late = avoid spring drought).

### Exercise D (20-Year Variability)

**Entities involved**: All; [[Clock]] spans 20 years; [[Weather]] varies yearly  
**Learning**: Rainfall variability across years drives ESW/yield variability; [[Manager]] fixed but outcomes variable.

---

## Questions Answered by This Source

### "How does APSIM work?"
→ See [[Clock]] daily sequence + component hierarchy

### "Why doesn't my crop sow?"
→ See [[Manager]] sowing rule + [[ESW_Extractable_Soil_Water]] threshold logic

### "What's the difference between LL and DUL?"
→ See [[Soil_Layers]] water characteristics table

### "How do I know if N is limiting vs. water?"
→ See [[Nitrogen_Uptake]] troubleshooting + [[Water_Balance]] ESW tracking

### "When should I apply fertilizer?"
→ See [[Manager]] fertilizer timing + [[Zadok_Scale]] critical periods + [[Nitrogen_Uptake]] stage-dependent demand

### "Why do I get different yields in different years?"
→ See [[Weather]] (rainfall variability) + [[Water_Balance]] (how rainfall becomes available water) + Exercise D (20-year analysis)

---

## For Instructors

**Use this source for**:
- Correcting student misconceptions (each entity has 4–5 misconceptions flagged)
- Troubleshooting "Why doesn't my simulation work?" → Diagnostic checklist in each entity
- Module planning (Modules 0–7 reference specific entities)
- Exercise design (each exercise uses subset of entities; see above)

**Example**: "Student says 'More fertilizer always means higher yield.'"  
→ Direct to [[Nitrogen_Uptake]] "Common Misconceptions" #1 + Exercise B (AEN response curve shows diminishing returns)

---

## Cross-References

### Related Concepts (Planned)

- **[[Scaffolding]]** — Why exercises are near-solved; pedagogic design
- **[[Near_Solved_Design]]** — How `.apsimx` files are pre-configured
- **[[Sowing_Window_Logic]]** — Details of [[Manager]] sowing rule decision-making
- **[[Water_Stress_Framework]]** — How [[Water_Balance]] drives crop stress responses
- **[[Phenology_Thermal_Time_Link]]** — Connection between [[Thermal_Time]] GDD and [[Zadok_Scale]] stages
- **[[AEN_Efficiency]]** — Framework for understanding [[Nitrogen_Uptake]] response (Exercise B)
- **[[Module_Dependencies]]** — How Modules 0–7 build on each entity

### Synthesis Pages (Planned)

- **N_Response_Teaching_Path** — Combines [[Manager]], [[Nitrogen_Uptake]], [[AEN_Efficiency]], Exercise B
- **Phenology_Sowing_Connection** — Combines [[Thermal_Time]], [[Zadok_Scale]], [[Manager]] sowing, Exercise C
- **Water_Stress_Timing** — Combines [[Water_Balance]], [[ESW_Extractable_Soil_Water]], [[Zadok_Scale]] critical periods
- **Regional_Variability_Across_Years** — Combines [[Weather]], [[Clock]] (20-year), Exercise D
- **Troubleshooting_Decision_Tree** — Navigation guide for common errors

---

## Status

✅ **Entities Created**: 11 pages (Clock, Weather, Soil, Soil_Layers, Manager, Report, DataStore, Wheat, Nitrogen_Uptake, Thermal_Time, Water_Balance, Zadok_Scale, ESW)

✅ **Source Summary**: This page

⏳ **Concept Pages**: 7 planned (not yet created)

⏳ **Synthesis Pages**: 5 planned (not yet created)

⏳ **FAQ Pages**: 3 planned (from student Q&A in teaching cycle)

---

## For Next Ingest

When adding next source (e.g., research paper on wheat phenology, N cycle, climate variability):
1. Read source
2. Extract relevant entities (may create new ones)
3. Update existing entity pages with citations
4. Create source summary page (like this one)
5. Update wiki/index.md
6. Add entry to wiki/log.md

---

## Metadata

| Property | Value |
|----------|-------|
| **Source file** | REFERENCE_DOCS/APSIM_Internal_Guide.md |
| **Lines** | 3000+ |
| **Ingest date** | 2026-05-01 |
| **Ingest time** | ~30 min |
| **Pages created** | 11 entity pages |
| **Pages updated** | 0 (all new) |
| **Wikilinks created** | ~80 cross-references |
| **Status** | Draft; awaiting concept page creation + synthesis |

