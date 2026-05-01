---
title: Report
tags: [entity, core-component, output]
related: [[DataStore]], [[Clock]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Report — Output Variable Selection

## Definition

The **Report** component defines which variables APSIM records each day. It acts as a filter: you choose which state variables (ESW, rainfall, grain mass, etc.) to output; unrequested variables are not saved.

**Key role**: Report is the gateway between simulation and analysis; without Report, simulation runs but produces no output.

---

## In APSIM

### Purpose

1. **Output selection**: Choose which variables to record
2. **Daily archiving**: Each day, record selected variables to DataStore
3. **Filtering**: Reduce output size (don't record variables you don't need)
4. **Naming**: Provide human-readable column names in output

### Configuration

**Report contains a list of variables** (typical):

```
[Report]
├─ [Clock].Today
├─ [Weather].Rainfall
├─ [Soil].SoilWater.ESW
├─ [Soil].NO₃
├─ [Wheat].Biomass
├─ [Wheat].Phenology.Stage
├─ [Wheat].GrainMass
└─ [Wheat].LAI
```

---

## Common Variable Paths

### Soil Variables

| Variable Path | Meaning | Units | Typical Range |
|---|---|---|---|
| `[Soil].SoilWater.ESW` | Extractable soil water | mm | 0–300 |
| `[Soil].NO₃(1)` | Nitrate in layer 1 | kg/ha | 0–200 |
| `[Soil].NH₄(1)` | Ammonium in layer 1 | kg/ha | 0–50 |
| `[Soil].SoilWater.Depth` | Total water profile | mm | 0–500 |

### Weather Variables

| Variable Path | Meaning | Units | Typical Range |
|---|---|---|---|
| `[Weather].Rainfall` | Daily rainfall | mm | 0–100 |
| `[Weather].MaxT` | Max temperature | °C | -10 to 50 |
| `[Weather].MinT` | Min temperature | °C | -20 to 35 |
| `[Weather].Radiation` | Solar radiation | MJ/m² | 0–30 |

### Crop Variables (Wheat)

| Variable Path | Meaning | Units | Typical Range |
|---|---|---|---|
| `[Wheat].Biomass` | Total above-ground biomass | kg/ha | 0–15000 |
| `[Wheat].GrainMass` | Grain mass | kg/ha | 0–8000 |
| `[Wheat].Yield` | Harvestable grain yield | kg/ha | 0–6000 |
| `[Wheat].LAI` | Leaf area index | m²/m² | 0–8 |
| `[Wheat].Stage` | Phenological stage (0–90+) | Zadok scale | 0–90+ |
| `[Wheat].DaysAfterSowing` | Days since sowing | days | 0–300 |

### Clock Variables

| Variable Path | Meaning | Units | Typical Range |
|---|---|---|---|
| `[Clock].Today` | Date of simulation | YYYY-MM-DD | - |

---

## Key Concepts

### Variable Naming Convention

APSIM uses **dot notation** to navigate component hierarchy:

```
[ComponentName].[SubComponent].[Variable]

Example: [Soil].SoilWater.ESW
  → "From Soil component, access SoilWater sub-component, get ESW"

Example: [Wheat].Phenology.Stage
  → "From Wheat component, access Phenology sub-component, get Stage"
```

### How Report Works

**Each day**:
1. Report reads variables from other components
2. Formats as a row in output table
3. Appends row to DataStore (in-memory database)

**At end of simulation**:
- DataStore exported to Excel (`.xlsx`) or CSV (`.csv`)
- Each row = one day
- Each column = one reported variable

---

## Output Data Structure

### Example: 7-day simulation

```
Date       | Rainfall(mm) | ESW(mm) | Wheat.Biomass(kg/ha) | Wheat.Stage
-----------|--------------|---------|----------------------|----------
2021-03-01 | 5.2          | 120     | 0                    | 0
2021-03-02 | 0.0          | 124     | 0                    | 0
2021-03-03 | 0.0          | 118     | 0.5                  | 5
2021-03-04 | 0.0          | 115     | 1.2                  | 5
2021-03-05 | 2.1          | 120     | 2.1                  | 10
2021-03-06 | 0.0          | 118     | 2.8                  | 10
2021-03-07 | 0.0          | 114     | 3.5                  | 15
```

---

## Common Misconceptions

### 1. "All variables are automatically recorded"
**Clarification**: Only variables listed in Report are saved. If you don't add `[Wheat].LAI`, LAI won't be available for analysis. Default Report usually includes only date, rainfall, and yield; you must add other variables.

### 2. "Variable name is just the component name (e.g., 'Biomass')"
**Clarification**: Must use full path including component name and sub-component: `[Wheat].Biomass`, not just `Biomass`.

### 3. "Recording more variables slows simulation down significantly"
**Clarification**: Recording extra variables has minimal performance impact (< 5% slowdown). Recording 10 vs. 50 variables makes little difference. Record what you need for analysis.

### 4. "I can change Report variables mid-simulation"
**Clarification**: Report configuration is set before simulation starts and cannot change during execution. To record different variables, you must re-run the simulation.

---

## Troubleshooting

### Report output is empty / zero output

**Checklist**:
1. Is Report inside the Simulation node? (Check `.apsimx` hierarchy; Report must be child of Simulation)
2. Is variable path spelled correctly? (Case-sensitive; `ESW` ≠ `esw`)
3. Is variable path valid? (Does the component have this sub-variable? Check APSIM documentation)
4. Did simulation actually run? (Check `.log` file for errors)

**Diagnostic**: Try adding a simple variable first:
```
[Clock].Today  (always available)
[Weather].Rainfall  (always available if Weather component exists)
```

### Variable returns "NaN" (Not a Number)

**Checklist**:
1. Is the crop planted? (Crop variables = 0 or NaN before sowing)
2. Is the variable defined for the component's current state? (e.g., `[Wheat].GrainMass` = 0 if in vegetative stage)
3. Is the path incomplete? (e.g., if path is `[Wheat].Biomass.Daily` but should be `[Wheat].Biomass`)

### Output file doesn't match expected number of days

**Checklist**:
1. Does Clock span full date range? (e.g., if Clock starts March 1 but ends March 10, only 10 days recorded)
2. Did simulation crash mid-run? (Check `.log` file)
3. Are missing days due to simulation stopping early? (Check Manager rules; might auto-harvest before expected date)

---

## Exercise References

- **Module 3** (Reading Output): Learn to interpret Report output
- **Module 4** (Reading Output continued): Plot Report data (ESW vs. rainfall, yield vs. N)
- **All exercises**: Analyze Report data to understand simulation outcomes

---

## Related Concepts

- **[[DataStore]]** — Receives data from Report; exports to Excel/CSV
- **[[Clock]]** — Report records once per day, synchronized with Clock
- **[[Soil]]**, **[[Wheat]]** — Components whose variables Report can access

---

## Resources

- **APSIM Documentation**: https://apsimnextgeneration.netlify.app/ → Report component
- **Variable browser**: APSIM includes tool to browse available variables (check documentation)
- **Example**: `wheat_example.apsimx` includes pre-configured Report with typical variables
