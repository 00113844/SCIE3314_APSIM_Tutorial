---
title: "Entity: Water Balance"
tags: [entity, soil, hydrology, module-2, module-3]
related: [[ESW_Extractable_Soil_Water]], [[Soil_Layers]], [[Runoff]], [[Drainage]], [[Wheat]], [[Water_Stress_Framework]]
last_updated: 2026-05-01
status: draft
---

# Water Balance

## Definition

The **water balance** is the daily accounting of all water entering, leaving, or stored in the soil:

$$
\text{ESW}_{t} = \text{ESW}_{t-1} + \text{Rain} - \text{Runoff} - \text{Drainage} - \text{Evaporation} - \text{Plant Uptake}
$$

where:
- **ESW** = extractable soil water (mm)
- **Rain** = daily rainfall (mm)
- **Runoff** = water lost due to high rainfall and low infiltration (mm)
- **Drainage** = water seeping below root zone (mm)
- **Evaporation** = direct soil evaporation + plant transpiration (mm)
- **Plant Uptake** = water extracted by roots (mm)

## Key Processes

### 1. Rainfall Input
- **Source**: Weather file (daily precipitation)
- **Fate**: Partitioned into:
  - Infiltration → increases ESW (up to DUL)
  - Runoff → lost to surface flow if infiltration capacity exceeded

### 2. Runoff
- **Occurs when**: Rainfall intensity > soil infiltration capacity
- **Depends on**: Soil texture (sandy soils infiltrate fast; clay slower), slope, residue cover
- **APSIM**: Uses infiltration method (CN curve or similar) to partition rain into infiltration vs runoff

### 3. Drainage
- **Occurs when**: ESW > DUL (soil saturated)
- **Fate**: Water drains downward (percolation) past root zone
- **Depends on**: Soil hydraulic conductivity, depth of LL, groundwater
- **Impact**: Nutrient leaching (N, P) if ESW stays high

### 4. Evaporation

#### Potential Evapotranspiration (PET)
Climate-driven potential; calculated from temperature, radiation, humidity, wind.

$$
\text{PET (mm/day)} = f(\text{T}_{\text{max}}, \text{T}_{\text{min}}, \text{Radiation}, \text{RH}, \text{Wind})
$$

#### Actual Evapotranspiration (AET)
Limited by available ESW:
- **If ESW > critical threshold**: AET ≈ PET (no water stress)
- **If ESW < critical**: AET reduced; stress factor limits plant uptake

### 5. Plant Uptake
- **Source**: Available water between LL and DUL (ESW)
- **Demand**: Based on leaf area, radiation, growth stage
- **Stress**: If ESW < critical, uptake reduced; yield suffers

## Daily Simulation Sequence

Each day in APSIM:

1. **Clock**: Record date
2. **Weather**: Read T_max, T_min, Rain, Radiation
3. **Manager**: Check sowing/fertilizer/harvest rules
4. **Soil.SoilWater**:
   - Calculate PET
   - Partition Rain → Infiltration vs Runoff
   - Update ESW: ESW += Infiltration − Drainage − Evaporation (base, no crop)
5. **Wheat** (if sown):
   - Calculate water demand
   - Extract water (uptake) from soil
   - Apply water stress factor if ESW < critical
6. **Report**: Log daily ESW, runoff, drainage, etc.

## Regional Water Balance Patterns

### Mediterranean Climate (AU South — Perth, Adelaide)
- **Summer** (Dec–Feb): Low rainfall, high PET → ESW ↓ to minimum
- **Winter** (Jun–Aug): High rainfall, low PET → ESW ↑ to maximum
- **Spring** (Sep–Nov): Moderate rain, increasing T → ESW variable; crop-critical period

### Subtropical Climate (AU East — Dalby, Brisbane)
- **Spring** (Sep–Nov): Increasing rain, T → ESW ↑; sowing window
- **Summer** (Dec–Feb): High rain but high PET; ESW may be stable or declining
- **Autumn** (Mar–May): Low rain, PET declining → ESW ↓
- **Winter** (Jun–Aug): Very low rain, low T → ESW ↓; some regions may have early rain

## Exercises Where Water Balance is Central

- [[Module_2_First_Simulation_Guide]]: Understand daily ESW updates in fallow
- [[Exercise_A_Fallow_Water_Balance_Guide]]: Plot ESW vs rain/runoff/drainage; identify seasonal pattern
- [[Module_3_Reading_Output_Guide]]: Extract water balance components from DataStore

## Common Student Misconceptions

### Misconception 1: "All rainfall becomes available soil water"
**Clarification**: Rainfall is partitioned into runoff (lost) and infiltration (stored). On days with high rain, much may run off.

### Misconception 2: "Evaporation is negligible; only plant transpiration matters"
**Clarification**: Evaporation is 20–40% of total water loss in most years. In fallow (no crop), direct evaporation is the only loss.

### Misconception 3: "Drainage is always bad"
**Clarification**: Some drainage is necessary (prevents waterlogging). Excessive drainage = N leaching risk. Moderate drainage is healthiest.

### Misconception 4: "Water balance is the same every year"
**Clarification**: No. Rainfall timing and amount vary yearly. A dry year has different ESW trajectory than a wet year.

## Troubleshooting

### Issue: ESW never changes (fallow simulation)
- **Check 1**: Rainfall = 0 in weather file?
  - **Fix**: Verify weather file covers typical wet season
- **Check 2**: Drainage too high? (Water disappears immediately)
  - **Fix**: Verify soil profile; check LL definition
- **Check 3**: Simulation too short? (Only a few days?)
  - **Fix**: Extend simulation to full year to see seasonal cycle

### Issue: ESW drops to zero immediately after sowing
- **Check 1**: High transpiration demand? (Plant grew large; uptake high?)
  - **Fix**: Check crop LAI; if MaxLAI reached early, plant may consume all ESW
- **Check 2**: Initial soil water too low?
  - **Fix**: Start with InitialWater = 0.6 (60% of available)

## Sources & References

- [[Holzworth_2014_APSIM_Summary]]: Soil water model equations
- [[APSIM_Internal_Guide.md]]: Section on SoilWater component and daily water balance
- [[Water_Stress_Framework]]: How water stress affects yield

---

**Last Updated**: May 1, 2026  
**Inbound Links**: [[Exercise_A_Fallow_Water_Balance_Guide]], [[Sowing_Window_Logic]], [[Water_Stress_Framework]]  
**Status**: Draft
