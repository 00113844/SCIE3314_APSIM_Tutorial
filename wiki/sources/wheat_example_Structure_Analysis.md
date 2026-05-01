---
title: wheat_example_Structure_Analysis
tags: [source, .apsimx-structure, simulation-design]
related: [[Manager]], [[Soil]], [[Report]], [[DataStore]], [[Clock]], [[Weather]]
source: [[wheat_example.apsimx]]
status: draft
---

# Source: wheat_example.apsimx — Structure Analysis

**Source File**: `Apsim_examples/wheat_example.apsimx` (JSON format, ~1100 lines)  
**Analysis Date**: May 1, 2026  
**Purpose**: Reference model for tutorial development; shows best-practice APSIM configuration

---

## Executive Summary

`wheat_example.apsimx` is a production-ready APSIM simulation template featuring:

- **Location**: Norwin, Darling Downs, Queensland (Lat -27.58°, Long 151.32°)
- **Soil**: Vertosol Black Clay (7 layers, 1950 mm total depth)
- **Crop**: Wheat (Hartog cultivar, 120 plants/m²)
- **Weather**: AU_Dalby.met (1900–2000, 101 years)
- **Management**: 3 rules (sowing, fertiliser, harvest)
- **N Strategy**: 160 kg/ha NO₃-N applied at sowing
- **Outputs**: 13 variables tracked daily (LAI, Zadok stage, biomass, grain mass, N content, yield)

**Key pedagogic value**: Shows how to structure a near-solved file (pre-configured components; students can modify parameters)

---

## File Structure Hierarchy

### JSON Structure (Top-Level)

```
{
  "$type": "Models.Core.Simulations",
  "Version": 195,
  "Name": "Simulations",
  "Children": [
    Simulation (1)
    DataStore (1)
  ]
}
```

**Two root children**:
1. **Simulation** — Main execution model (contains all components)
2. **DataStore** — Output database (receives daily records from Report)

---

## Component Hierarchy (Inside Simulation)

### Global Components (non-spatial)

```
Simulation/
├─ Clock                       (start: 1900-01-01, end: 2000-12-31)
├─ Summary                     (logging/verbosity control)
├─ Weather                     (file: AU_Dalby.met)
├─ SoilArbitrator             (water/N coordination)
└─ MicroClimate               (radiation interception, soil heat flux)
```

### Zone (Spatial - represents "Field")

```
Field/ (Area=1.0 hectare, Altitude=50m)
├─ Report                      (output variables)
├─ Fertiliser                  (N/P/K application component)
├─ Soil                        (profile with 7 layers)
│  ├─ Physical                (BD, LL, DUL, SAT, KS for each layer)
│  │  └─ SoilCrop(WheatSoil)  (crop-specific LL, KL, XF)
│  ├─ SoilWater               (WaterBalance; evaporation, drainage params)
│  ├─ Organic                 (Carbon, FOM pools, decay rates)
│  ├─ Chemical                (pH, EC, ESP)
│  ├─ Water                   (initial water content by layer)
│  ├─ Nutrient                (N cycling model)
│  ├─ NO3                     (nitrate pool; initial 1.0 kg/ha)
│  ├─ NH4                     (ammonium pool; initial 0.1 kg/ha)
│  ├─ Urea                    (urea pool; initial 0.0 kg/ha)
│  └─ Temperature             (soil temperature calc)
├─ SurfaceOrganicMatter        (residue: 500 kg/ha wheat stubble, CNR=100)
├─ Wheat                       (PMF.Plant; crop model with phenology, growth)
├─ Manager: "Sow using variable rule"    (JavaScript; ESW + rain logic)
├─ Manager: "Fertilise at sowing"        (JavaScript; 160 kg/ha NO₃-N)
└─ Manager: "Harvest"                    (JavaScript; harvest when ready)

Graph                         (visualization; grain mass vs. date)
DataStore                     (output database)
```

---

## Clock Configuration

```json
{
  "$type": "Models.Clock, Models",
  "Start": "1900-01-01T00:00:00",
  "End": "2000-12-31T00:00:00",
  "Name": "Clock"
}
```

**Key parameters**:
- **Start date**: 1900-01-01 (matches weather file start)
- **End date**: 2000-12-31 (101 years; can run long-term scenarios)
- **Timestep**: Daily (hard-coded; not configurable)

**Pedagogic note**: Span is 101 years — allows multi-year variability analysis; Exercise D (20-year) will use subset of this.

---

## Weather Configuration

```json
{
  "$type": "Models.Climate.Weather, Models",
  "FileName": "%root%/Examples/WeatherFiles/AU_Dalby.met",
  "Name": "Weather"
}
```

**Key parameters**:
- **File path**: Relative path using `%root%` variable (portable)
- **Format**: `.met` file (SILO/BOM format)
- **Data**: Daily T_max, T_min, rainfall, radiation for Dalby, Queensland

**Pedagogic note**: Shows how to link external data; students can substitute different weather files.

---

## Soil Profile Structure

### Physical Properties (7 Layers)

| Layer | Depth (mm) | Thickness | BD | LL15 | DUL | SAT | KS |
|-------|------------|-----------|----|----- |-----|-----|-----|
| 1 | 0–150 | 150 | 1.011 | 0.261 | 0.521 | 0.589 | 20 |
| 2 | 150–300 | 150 | 1.071 | 0.248 | 0.497 | 0.566 | 20 |
| 3 | 300–600 | 300 | 1.094 | 0.280 | 0.488 | 0.557 | 20 |
| 4 | 600–900 | 300 | 1.159 | 0.280 | 0.480 | 0.533 | 20 |
| 5 | 900–1200 | 300 | 1.173 | 0.280 | 0.472 | 0.527 | 20 |
| 6 | 1200–1500 | 300 | 1.163 | 0.280 | 0.457 | 0.531 | 20 |
| 7 | 1500–1950 | 300 | 1.187 | 0.280 | 0.452 | 0.522 | 20 |

**Key observations**:
- Soil is Vertosol (Black Clay) — sticky, high water-holding capacity
- LL increases with depth (clay subsoil has more bound water; harder for crop to extract)
- DUL decreases with depth (paradoxical but realistic for clay; macropore drainage more efficient)
- KS uniform at 20 mm/day (well-drained soil)
- Total plant-available water (PAW): ~361 mm (high; excellent for wheat)

### Soil Crop (Wheat-Specific)

```json
{
  "Name": "WheatSoil",
  "LL": [0.261, 0.248, 0.28, 0.306, 0.36, 0.392, 0.446],
  "KL": [0.06, 0.06, 0.06, 0.04, 0.04, 0.02, 0.01],
  "XF": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0]
}
```

**Parameters**:
- **LL** (per-crop Lower Limit): Crop-specific; may differ from physical LL
- **KL** (extraction coefficient): Rate of water extraction per layer; higher near surface (easier access)
- **XF** (exploration fraction): 1.0 = full root access; reflects root growth

**Pedagogic note**: Shows how crop-specific parameters override physical soil properties.

### Water Balance (SoilWater)

```json
{
  "SummerDate": "1-Nov",
  "SummerU": 5.0,
  "SummerCona": 5.0,
  "WinterDate": "1-Apr",
  "WinterU": 5.0,
  "WinterCona": 5.0,
  "DiffusConst": 40.0,
  "DiffusSlope": 16.0,
  "Salb": 0.12,
  "CN2Bare": 73.0,
  "SWCON": [0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3]
}
```

**Key parameters**:
- **U (summer/winter)**: Evaporation demand reduction parameter
- **Cona**: Evaporation from soil surface
- **DiffusConst/Slope**: Diffusivity parameters for water redistribution
- **Salb**: Soil albedo (0.12 = dark clay)
- **CN2Bare**: USDA Curve Number for runoff calculation
- **SWCON**: Conductivity for drainage between layers

**Pedagogic note**: Water Balance is complex; students should understand conceptually (PET, runoff, drainage) but parameters are pre-set.

### Organic Matter & Nitrogen

**Organic Carbon** (by layer):
```
Carbon (% by mass): [1.2, 0.96, 0.6, 0.3, 0.18, 0.12, 0.12]
```
- Topsoil (Layer 1) richest in organic matter
- Rapidly decreases with depth (typical pattern)

**Initial N Pools**:
- **NO₃**: 1.0 kg/ha per layer (total ~7 kg/ha)
- **NH₄**: 0.1 kg/ha per layer (total ~0.7 kg/ha)
- **Urea**: 0.0 kg/ha (added at sowing via fertiliser rule)

**Pedagogic note**: N pools are low initially; fertiliser (160 kg/ha NO₃-N) will dominate crop N supply in this scenario.

### Initial Water Content

```json
{
  "InitialValues": [0.521, 0.497, 0.488, 0.480, 0.472, 0.457, 0.452],
  "InitialPAWmm": 361.2454283127387,
  "RelativeTo": "LL15"
}
```

**Key parameters**:
- **InitialValues**: Water content as fraction at DUL (soil is "full" at start)
- **InitialPAWmm**: 361 mm plant-available water (excellent; ready for sowing)
- **RelativeTo**: Reference (LL15 = wilting point; this is standard APSIM)

**Pedagogic note**: Simulation starts with soil at capacity — ideal for sowing; no need to wait for rainfall.

---

## Report Configuration

### Variables Tracked (13 daily outputs)

```json
"VariableNames": [
  "[Clock].Today",
  "[Wheat].LAI",
  "[Wheat].Phenology.Zadok.Stage",
  "[Wheat].Phenology.CurrentStageName",
  "[Wheat].AboveGround.Wt",
  "[Wheat].AboveGround.N",
  "[Wheat].Grain.Total.Wt*10 as Yield",     // *10 for kg/ha
  "[Wheat].Grain.Protein",
  "[Wheat].Grain.Size",
  "[Wheat].Grain.Number",
  "[Wheat].Grain.Total.Wt",
  "[Wheat].Grain.Total.N",
  "[Wheat].Total.Wt"
],
"EventNames": [
  "[Wheat].Harvesting"                       // Log harvest event
]
```

**Pedagogic value**:
- Shows proper variable naming convention (e.g., `[Wheat].Grain.Total.Wt`)
- Includes phenological tracking (Zadok stage, stage name)
- Includes N cycling (Grain.N, AboveGround.N)
- Includes grain metrics (Number, Size, Protein)
- Event logging (harvest) for decision analysis

---

## Manager Rules (JavaScript)

### Rule 1: "Sow using a variable rule"

**Parameters**:
```json
{
  "Crop": "Wheat",
  "StartDate": "1-may",
  "EndDate": "10-jul",
  "MinESW": 100.0,
  "MinRain": 25.0,
  "RainDays": 7,
  "CultivarName": "Hartog",
  "SowingDepth": 30.0,
  "RowSpacing": 250.0,
  "Population": 120.0
}
```

**Logic** (JavaScript):
```javascript
if (DateUtilities.WithinDates(StartDate, Clock.Today, EndDate) &&
    !Crop.IsAlive &&
    MathUtilities.Sum(waterBalance.ESW) > MinESW &&
    accumulatedRain.Sum > MinRain)
{
    Crop.Sow(population: Population, cultivar: CultivarName, 
             depth: SowingDepth, rowSpacing: RowSpacing);
}
```

**Translation**: "Between May 1 and July 10, if crop is not alive AND ESW > 100 mm AND rainfall in last 7 days > 25 mm, then sow."

**Pedagogic design**:
- **Parametrized**: All values are in Parameters section (easy to modify)
- **Near-solved**: Students don't need to write JavaScript; they just change parameters
- **Decision-based**: Teaches sowing logic (ESW + rain threshold) without code complexity

### Rule 2: "Fertilise at sowing"

**Parameters**:
```json
{
  "Crop": "Wheat",
  "FertiliserType": "NO3N",
  "Amount": 160.0
}
```

**Logic**:
```javascript
[EventSubscribe("Sowing")]
private void OnSowing(object sender, EventArgs e)
{
    if (Crop != null && crop.Name.ToLower() == (Crop as IModel).Name.ToLower())
        Fertiliser.Apply(amount: Amount, type: FertiliserType);
}
```

**Translation**: "When crop is sown, apply 160 kg/ha NO₃-N."

**Pedagogic design**:
- Event-driven (fires on crop sowing event, not calendar date)
- Shows N form choice (NO3N = immediately available)
- Single application at sowing (allows Exercise B exploration: early vs. split timing)

### Rule 3: "Harvest"

**Parameters**:
```json
{
  "Crop": "Wheat"
}
```

**Logic**:
```javascript
[EventSubscribe("DoManagement")]
private void OnDoManagement(object sender, EventArgs e)
{
    if (Crop.IsReadyForHarvesting)
    {
        Crop.Harvest();
        Crop.EndCrop();
    }
}
```

**Translation**: "When crop is ready for harvesting (physiological maturity), harvest and end the crop season."

**Pedagogic design**:
- Automatic (no manual date needed; uses crop's internal phenology)
- Ensures harvest at optimal time (maximum grain fill)
- Removes guesswork; students focus on sowing/N decisions

---

## Outputs & Visualization

### Report Output (DataStore)

**Daily table structure**:
```
Date       | LAI | Zadok | Biomass | Grain.N | Yield | ...
-----------|-----|-------|---------|---------|-------|----
1900-01-01 | 0   | 0     | 0       | 0       | 0     |
...        |     |       |         |         |       |
1950-03-20 | 0   | 0     | 0       | 0       | 0     | (sowing day)
1950-03-30 | 0.5 | 5     | 2.1     | 0       | 0     | (emergence)
...        |     |       |         |         |       |
1950-11-20 | 0   | 90    | 12500   | 190     | 5200  | (harvest)
```

**Exported to Excel/CSV** for student analysis.

### Graph Configuration

**X-axis**: Date  
**Y-axis**: Wheat.Grain.Total.Wt (grain mass accumulation over time)  
**Table**: Report  

**Pedagogic value**: Shows how to visualize simulation output; students will generate similar graphs in Module 3.

---

## Design Principles for Tutorial Development

### 1. Near-Solved File Template

**What's pre-configured**:
- ✅ Soil profile (realistic 7-layer Vertosol)
- ✅ Weather file (101-year climate data)
- ✅ Manager rules (parametrized; no code writing needed)
- ✅ Crop model (Hartog wheat with standard parameters)
- ✅ Report (key 13 variables already selected)

**What students modify** (for exercises):
- Sowing parameters (date window, ESW threshold, population)
- N rate (Exercise B: 0, 80, 160, 240 kg/ha)
- Sowing date (Exercise C: vary StartDate/EndDate)
- Multi-year simulation (Exercise D: Clock stays 1900–2000)

**Why this works**: Reduces cognitive load; students focus on concepts (decision rules, N response, phenology) not file editing.

### 2. Realistic Soil Profile

**Vertosol Black Clay** (actual APSOIL record #900):
- Represents Australian upland agricultural soils
- High water-holding capacity (PAW = 361 mm)
- Realistic depth (1950 mm; includes deep layers for long-term scenarios)
- Layer structure models texture variation (topsoil → subsoil)

**Pedagogic value**: Students learn on real soil data; not synthetic/simplified.

### 3. Parametrized Manager Rules

**All decision thresholds are parameters**:
- MinESW = 100 mm (easy to change)
- MinRain = 25 mm over 7 days (easy to change)
- Fertiliser amount = 160 kg/ha (easy to change)

**Why**: Students can explore sensitivity without coding:
- "What if we required ESW > 150 mm?" → Change parameter, re-run
- "What if we applied 240 kg/ha N?" → Change parameter, re-run
- "What if sowing window was May–August?" → Change dates, re-run

### 4. Multi-Scale Temporal Design

**Clock spans 101 years (1900–2000)** but single simulation by default:
- Exercise A (Fallow): Year 1 only (test water balance)
- Exercise B (N Response): Single season (1949–1950 or other single year)
- Exercise D (20-Year): Subset of 101 years (e.g., 1950–1969)

**Why**: One .apsimx file can support multiple exercise pedagogies by selecting different Clock ranges.

### 5. Report Focus on Agronomic Metrics

**13 variables balance detail + simplicity**:
- **Phenology**: Zadok stage, stage name (understand development)
- **Growth**: LAI, total biomass, grain mass (track progression)
- **Grain quality**: Grain number, size, protein, N content (understand quality factors)
- **Yield**: Grain total weight × 10 as yield (final metric)

**Why**: Students see full crop life cycle, not just output.

---

## Comparison to Tutorial Needs

### Module 1 (Structure): Use as Reference

Show students the [[Clock]] → Weather → Soil → Wheat → Manager → Report hierarchy using this file.

### Module 2 (First Simulation): Simplified Fallow Version

Create Exercise A by removing:
- Wheat component
- Manager (sowing/fertiliser/harvest rules)
- Crop-specific Report variables

Keep: Clock, Weather, Soil, Report (with [Soil].ESW tracking)

### Module 3–4 (Output Analysis): Export and Analyze

Run wheat_example.apsimx (or Exercise B variant); export to Excel; students create plots, analyze N response, calculate AEN.

### Module 5–6 (Phenology & N): Modify Parameters

- Exercise B: Clone wheat_example.apsimx; create 4 versions (N = 0, 80, 160, 240 kg/ha)
- Exercise C: Clone; modify StartDate/EndDate (sowing date factorial)

### Module 7 (Long-Term): Use Full 101-Year Clock

Run simulation with full date range; students analyze yield distribution, rainfall correlation over 20–40 year subsets.

---

## File Modification Workflow

### For Exercise A (Fallow)
```
1. Copy wheat_example.apsimx → Exercise_A_Fallow.apsimx
2. Remove: Wheat, Fertiliser, 3 Manager rules
3. Modify Report: keep only [Clock].Today, [Soil].ESW, [Weather].Rainfall
4. Modify Clock: set to single year (e.g., 1950-01-01 to 1950-12-31)
5. Students run; analyze daily ESW vs. rainfall
```

### For Exercise B (N Response)
```
1. Copy wheat_example.apsimx 4 times:
   - Exercise_B_N0.apsimx
   - Exercise_B_N80.apsimx
   - Exercise_B_N160.apsimx (keep as-is; amount: 160.0)
   - Exercise_B_N240.apsimx
2. In each, modify Manager "Fertilise at sowing" → change Amount parameter
3. Students run all 4; compare yields; calculate AEN
```

### For Exercise C (Sowing Date)
```
1. Copy wheat_example.apsimx 3 times:
   - Exercise_C_EarlyMay.apsimx (StartDate: 1-May, EndDate: 10-Jun)
   - Exercise_C_MidJun.apsimx (StartDate: 15-Jun, EndDate: 30-Jul)
   - Exercise_C_LateJul.apsimx (StartDate: 1-Aug, EndDate: 31-Aug)
2. In each, modify Manager "Sow" → change StartDate/EndDate
3. Students run all 3; compare phenology, yield, grain protein
```

---

## Metadata

| Property | Value |
|----------|-------|
| **File** | wheat_example.apsimx |
| **Format** | JSON (APSIM Next Gen format) |
| **Size** | ~1100 lines |
| **Location** | Norwin, Darling Downs, Queensland |
| **Soil** | Vertosol Black Clay (APSOIL #900) |
| **Crop** | Wheat (Hartog cultivar) |
| **Climate** | AU_Dalby.met (101 years: 1900–2000) |
| **N Strategy** | 160 kg/ha NO₃-N at sowing |
| **Yield Estimate** | ~5200 kg/ha (based on Report structure) |
| **Pedagogic Grade** | ★★★★★ (excellent template; near-solved; parametrized) |

---

## Recommendations for Tutorial Development

1. **Use wheat_example.apsimx as master template** — all exercises should be derived from it
2. **Create parametrized variants** — Exercise A, B, C, D as copies with Manager parameters modified
3. **Document parameter meanings** — explain ESW threshold, N rate, sowing date logic for each exercise
4. **Provide side-by-side comparisons** — show how changing one parameter affects outcomes
5. **Link to wiki entities** — each modified file should reference relevant wiki pages ([[Manager]], [[ESW_Extractable_Soil_Water]], [[Nitrogen_Uptake]], [[Zadok_Scale]])

---

## See Also

- [[CLAUDE.md]] → Page Template: Exercise (structure for exercise guides)
- [[Manager]] → Understand Manager rule structure + parametrization
- [[Report]] → Understand Report output variable selection
- [[Soil]] → Understand soil profile configuration
- [[Exercise_A_Fallow_Water_Balance_Guide]] (to be created; will reference this file)
- [[Exercise_B_Nitrogen_Response_Guide]] (to be created; will reference this file)
