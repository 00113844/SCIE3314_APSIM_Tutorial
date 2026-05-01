---
title: "Entity: ESW (Extractable Soil Water)"
tags: [entity, soil-water, module-2, module-3]
related: [[Water_Balance]], [[Soil_Layers]], [[Runoff]], [[Sowing_Window_Logic]]
last_updated: 2026-05-01
status: draft
---

# ESW: Extractable Soil Water

## Definition

**Extractable Soil Water (ESW)** is the amount of water in the soil that plants can extract between two critical thresholds:

- **DUL** (Drained Upper Limit) — water content after soil drains (field capacity)
- **LL** (Lower Limit) — water content when plants can no longer extract water (wilting point)

$$
\text{ESW (mm)} = \sum_{\text{layers}} (\theta_{\text{DUL}} - \theta_{\text{LL}}) \times \text{Thickness}
$$

where θ = volumetric water content (mm water per mm soil).

**Note**: ESW is layer-specific. A 100 mm thick layer with 0.30 mm/mm water content × DUL − LL gives ESW = 30 mm for that layer. Total ESW = sum across all layers.

## Role in APSIM

ESW is critical for two processes:

### 1. Sowing Decision Logic
Farmers use ESW to time sowing:
- **Threshold**: ESW ≥ 100 mm typical for wheat (varies by region/crop)
- **Rationale**: Enough moisture for germination + seedling establishment without waterlogging
- **APSIM Manager rule**: `IF (ESW >= 100 AND rain_7day >= 25) THEN sow()`

### 2. Water Stress During Growth
Once crop is sown:
- **If ESW > critical**: Plant can extract water freely; no stress
- **If ESW between critical and LL**: Plant experiences water stress; growth reduced
- **If ESW < LL**: Plant dies or stops growing

## Related Processes

| Process | How ESW Changes |
|---------|-----------------|
| **Rainfall** | ESW ↑ (up to DUL; excess → runoff) |
| **Runoff** | ESW ↓ (water lost from soil) |
| **Drainage** | ESW ↓ (water seeps below LL) |
| **Evaporation** | ESW ↓ (direct soil evaporation) |
| **Plant uptake** | ESW ↓ (crop extraction) |

## Daily ESW Update (Water Balance)

$$
\text{ESW}_{t} = \text{ESW}_{t-1} + \text{Rainfall} - \text{Runoff} - \text{Drainage} - \text{Evaporation} - \text{Plant uptake}
$$

## Exercises Where ESW is Central

- [[Exercise_A_Fallow_Water_Balance_Guide]]: Plot ESW over one year; observe seasonal pattern
- [[Module_2_First_Simulation_Guide]]: Understand daily ESW updates
- [[Module_3_Reading_Output_Guide]]: Extract ESW from DataStore; graph time series

## Common Student Misconceptions

### Misconception 1: "ESW is the total water in the soil"
**Clarification**: No. ESW is only the plant-available portion between DUL and LL. Water above DUL (saturated) and below LL (unavailable) are not part of ESW.

### Misconception 2: "ESW is constant throughout the year"
**Clarification**: ESW changes daily based on rainfall, evaporation, drainage, and plant uptake. In winter it may be high; in summer it decreases.

### Misconception 3: "More rainfall always increases ESW"
**Clarification**: Rainfall increases ESW only if it doesn't exceed DUL (max available) or cause runoff. High rainfall on already-saturated soil → runoff, not more ESW.

### Misconception 4: "If ESW drops below LL, the soil is dry"
**Clarification**: The soil still has water below LL; plants just can't extract it. Below LL = water is bound to soil particles too tightly.

## Troubleshooting

### Issue: Sowing rule never triggers (ESW never reaches 100 mm)
- **Check**: Regional rainfall pattern; typical ESW at sowing time?
- **Check**: Initial soil water set too low? (Try InitialWater = 0.5 = 50% of available)
- **Check**: Threshold too high for region? (Try 80 mm instead of 100)

### Issue: ESW doesn't change; stuck at initial value
- **Check**: Rainfall = 0 in weather file?
- **Check**: Soil drainage too high? (Water immediately drains)
- **Check**: Simulation is fallow (no evaporation/uptake source)?

## Sources & References

- [[Holzworth_2014_APSIM_Summary]]: Soil water balance equations
- [[APSIM_Internal_Guide.md]]: Section on SoilWater component

---

**Last Updated**: May 1, 2026  
**Inbound Links**: [[Sowing_Window_Logic]], [[Water_Balance]], [[Exercise_A_Fallow_Water_Balance_Guide]]  
**Status**: Draft
