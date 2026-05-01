---
title: "Entity: Wheat Crop Model"
tags: [entity, crop-model, module-1, module-5]
related: [[Phenology]], [[Thermal_Time]], [[Zadok_Scale]], [[Nitrogen_Uptake]], [[Water_Balance]]
last_updated: 2026-05-01
status: draft
---

# Wheat Crop Model

## Definition

The **Wheat** component in APSIM is a process-based crop model simulating daily phenological development, biomass accumulation, and grain yield in response to climate, soil, and management. It operates on a daily timestep, converting daily weather inputs (temperature, radiation, rainfall) into crop state variables (stage, biomass, leaf area).

## Key Parameters

| Parameter | Typical Range | Notes |
|-----------|---------------|-------|
| **Cultivar** | Hartog, Yitpi, etc. | Determines GDD milestones for each phenological stage |
| **MaxLAI** | 6–8 m²/m² | Maximum leaf area index; limits photosynthesis |
| **GrainFillDuration** | 25–35 days | Days from anthesis to physiological maturity |
| **Vernalization** | Yes (winter wheat) / No (spring wheat) | Cold requirement for flower induction |
| **T_base** | 0–4°C | Base temperature for thermal time accumulation |

## Role in APSIM

The Wheat component drives daily simulation:

1. **Phenological development**: Thermal time accumulates each day; stage progresses (emergence → booting → anthesis → grain fill → maturity)
2. **Photosynthesis**: Green leaf area × radiation → biomass growth
3. **Partitioning**: Biomass allocated to roots, vegetative shoots, or grain (depends on stage)
4. **Water uptake**: Roots extract available water; stress factor reduces growth if ESW critical
5. **N uptake**: Takes up soil N; stress factor reduces growth if N limiting
6. **Yield**: Grain mass at harvest = f(thermal time, water, N)

## Key Equations

### Thermal Time Accumulation

$$
\text{GDD} = \max(0, \min(\text{T}_{\text{max}}, \text{T}_{\text{opt}}) - \text{T}_{\text{base}})
$$

where:
- T_max, T_min = daily max/min temperature
- T_base = base temperature (~0°C for wheat, higher post-vernalization)
- T_opt = optimal temperature (~20°C for vegetative growth)

### Phenological Stages (Zadok Scale)

| Zadok | Stage | Typical GDD from Sowing | Critical Period? |
|-------|-------|------------------------|------------------|
| 0–10 | Germination/Emergence | 50–80 | — |
| 20–30 | Leaf development | 150–250 | — |
| 31–39 | Stem elongation | 300–600 | Water-sensitive |
| 45–49 | Booting | 600–700 | — |
| 50–59 | Inflorescence emergence/Heading | 700–850 | Frost-sensitive |
| 60–69 | Anthesis/Pollination | 850–1000 | Critical: water, heat |
| 71–89 | Grain development/Maturation | 1000–1300 | — |
| 90+ | Ripeness | — | Harvest ready |

## Related Concepts

- [[Phenology]]: Stage classification and how it affects yield
- [[Thermal_Time]]: GDD accumulation across environments
- [[Zadok_Scale]]: Standardized stage naming
- [[Water_Balance]]: ESW availability affects wheat growth
- [[Nitrogen_Uptake]]: N demand vs. soil N supply
- [[Sowing_Window_Logic]]: When to plant; frost/heat risk timing

## Exercises Where Wheat is Central

- [[Module_1_Structure_Guide]]: Explore Wheat component in APSIM interface
- [[Module_5_Phenology_Guide]]: Thermal time & Zadok stages; sowing date effects
- [[Exercise_C_Sowing_Date_Phenology_Guide]]: Sowing date factorial; frost/heat risk windows
- [[Exercise_B_Nitrogen_Response_Guide]]: N fertiliser effects on grain yield

## Common Student Misconceptions

### Misconception 1: "Wheat always needs exactly 1000 GDD to mature"
**Clarification**: GDD milestones vary by cultivar, location, and temperature patterns. Hartog wheat typically ~1100 GDD from sowing to maturity, but this changes if temperatures are consistently above/below optimum.

### Misconception 2: "More leaves = always higher yield"
**Clarification**: LAI affects photosynthesis, but only up to MaxLAI (~6–8). Beyond that, leaves shade each other and don't contribute. Excess leaf growth wastes N and water.

### Misconception 3: "Anthesis (flowering) is where the grain comes from"
**Clarification**: Anthesis is pollination. Grain develops during the grain-fill stage (after anthesis). If the crop aborts flowers due to water/heat stress at anthesis, no grain forms later.

### Misconception 4: "Wheat yield is determined entirely by temperature"
**Clarification**: Yield is f(temperature, water, N, management, cultivar). A warm season with low rainfall still produces low yield if water is limiting.

## Troubleshooting

### Issue: Wheat won't emerge
- **Check**: Soil temperature ≥4°C?
- **Check**: ESW ≥50 mm (seed needs moisture)?
- **Check**: Sowing rule firing? (ESW + rainfall thresholds met?)
- **Action**: If checks fail, adjust initial water or sowing rule

### Issue: Grain yield = 0
- **Check**: Crop emerged and grew?
- **Check**: Harvest rule fired at/after physiological maturity?
- **Check**: Grain mass being tracked? (`[Wheat].Grain.Wt` in Report?)
- **Action**: Verify harvest timing; ensure Report includes grain mass

### Issue: Thermal time stuck (phenology not advancing)
- **Check**: Temperature outside cardinal range? (Too cold? Too hot?)
- **Check**: Vernalization requirement (winter wheat) met?
- **Action**: Check weather data; verify cultivar vernalization setting

## Sources & References

- [[Holzworth_2014_APSIM_Summary]]: Full crop model description (APSIM Next Gen)
- [APSIM Next Gen Documentation](https://apsimnextgeneration.netlify.app/): Wheat module details
- [Zadok Scale Reference](https://en.wikipedia.org/wiki/Feekes_scale): Phenological stage classification

---

**Last Updated**: May 1, 2026  
**Inbound Links**: [[Exercise_C_Sowing_Date_Phenology_Guide]], [[Module_5_Phenology_Guide]], [[Water_Stress_Framework]]  
**Status**: Draft (awaiting source integration)
