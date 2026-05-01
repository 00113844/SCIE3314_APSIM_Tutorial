# SCIE3314 APSIM Wiki — Index

**Last Updated**: May 1, 2026  
**Total Pages**: 28  
**Total Sources Ingested**: 0  
**Image Placeholders**: 0  

---

## Entities (APSIM Components & Concepts)

Core APSIM simulation components and key variables.

- [[Wheat]] — Wheat crop model; phenology, growth, yield
- [[ESW_Extractable_Soil_Water]] — Plant-available water between DUL and LL
- [[Manager_Component]] — Sowing, fertilizer, harvest logic
- [[Soil_Layers]] — Soil profile structure and water characteristics
- [[Thermal_Time]] — Growing degree days (GDD); phenological development
- [[Zadok_Scale]] — Standardized phenology stage classification
- [[Water_Balance]] — Daily rainfall, runoff, drainage, evaporation
- [[Nitrogen_Uptake]] — Plant N demand; soil N availability
- [[Clock_Component]] — Simulation timing (start/end dates)
- [[Weather_Component]] — Daily climate inputs
- [[Report_Component]] — Output variable specification
- [[DataStore_Component]] — Simulation results database

---

## Concepts (Pedagogic Frameworks)

Teaching and learning design principles.

- [[Scaffolding_Strategy]] — Why near-solved files reduce cognitive load; Bloom's progression
- [[Near_Solved_File_Design]] — How to create `.apsimx` files; what students configure vs. pre-configured
- [[Sowing_Window_Logic]] — Decision tree for ESW + rainfall thresholds + date windows
- [[Water_Stress_Framework]] — When water limits yield; critical periods for crops
- [[Phenology_Thermal_Time_Link]] — Why thermal time predicts development across environments
- [[AEN_Agronomic_Efficiency_Nitrogen]] — N response methodology and interpretation
- [[Module_Dependencies_Roadmap]] — 8-module learning progression and prerequisites

---

## Sources (Ingested)

Summaries of ingested APSIM docs, research papers, pedagogic guides.

*[To be populated as sources are added]*

---

## Synthesis (Cross-Cutting Analysis)

Analysis spanning multiple sources, concepts, and exercises.

- [[N_Response_Teaching_Path]] — Full pathway: Exercise B → N response concept → AEN calculation → risk management
- [[Phenology_Sowing_Connection]] — How thermal time + phenology inform sowing window decisions
- [[Water_Stress_Risk_Framework]] — Integrate ESW, rainfall, PET, root depth into risk assessment
- [[Module_0_7_Learning_Arc]] — How 8 modules build knowledge; conceptual progression
- [[Exercise_A_to_D_Narrative]] — How exercises A–D build integrated understanding

---

## FAQ (Troubleshooting & Q&A)

Student questions and troubleshooting guides.

- [[Crop_Won't_Emerge]] — Diagnostic decision tree: sowing rule, ESW, temperature, date window
- [[Yield_Unrealistic]] — Checks for N, water, phenology, harvest timing
- [[Report_Variables_Empty]] — Variable naming, parent relationships, simulation run status

---

## Exercises

### Modular Exercises (Modules 0–7)

Each module is 30 min to 1.5 hrs and stands alone or builds on prerequisites.

- [[Module_0_Why_APSIM_Guide]] — 30 min; motivation for process-based modeling
- [[Module_1_Structure_Guide]] — 60 min; APSIM interface and component hierarchy
- [[Module_2_First_Simulation_Guide]] — 60 min; fallow water balance (connects to Exercise A)
- [[Module_3_Reading_Output_Guide]] — 60 min; DataStore + graphing (connects to Exercise A output)
- [[Module_4_Nitrogen_Fertilisation_Guide]] — 60 min; factorial experiment (connects to Exercise B)
- [[Module_5_Phenology_Guide]] — 60 min; thermal time and Zadok stages (connects to Exercise C)
- [[Module_6_Long_Term_Variability_Guide]] — 90 min; 20-year factorial, risk assessment (connects to Exercise D)
- [[Module_7_Climate_Scenarios_Guide]] — 60–90 min; weather modification, sensitivity analysis (optional)

### Integrated Exercises (A–D)

Hands-on simulations spanning 2–4 hours total; can be taught as standalone or as arc.

- [[Exercise_A_Fallow_Water_Balance_Guide]] — 30 min; ESW time series, seasonal runoff
- [[Exercise_B_Nitrogen_Response_Guide]] — 60 min; factorial N rates, AEN calculation
- [[Exercise_C_Sowing_Date_Phenology_Guide]] — 60 min; sowing date factorial, frost/heat risk
- [[Exercise_D_20Year_Variability_Guide]] — 60 min; long-term uncertainty, yield vs. N × sowing date

---

## Image Placeholders

Specifications for diagrams to be generated later.

*[To be populated as wiki pages are created]*

---

## Recent Activity

See `log.md` for full ingest and query history.

**Latest**: 
- Bootstrap 1: Schema + folder structure created (May 1, 2026)

---

## Navigation Tips

1. **New to APSIM?** Start with [[Wheat]] or [[Module_0_Why_APSIM_Guide]]
2. **Teaching an exercise?** See [[Exercise_A_Fallow_Water_Balance_Guide]] (or B, C, D)
3. **Student troubleshooting?** Check [[FAQ]] sections
4. **Understanding concepts?** Read [[Concepts]] then related [[Entities]]
5. **Finding connections?** See [[Synthesis]] pages for cross-cutting analysis

---

**Next Update**: After first source ingest
