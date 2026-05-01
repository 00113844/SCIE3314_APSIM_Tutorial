---
title: Weather
tags: [entity, core-component, input-data]
related: [[Clock]], [[Soil]], [[Crop]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Weather — Daily Climate Data Input

## Definition

The **Weather** component reads and supplies daily meteorological data (temperature, rainfall, radiation, humidity) from a `.met` file (SILO or Bureau of Meteorology format). It provides the atmospheric drivers for all simulation processes.

**Key role**: Weather is the primary external forcing function; all daily Soil and Crop processes depend on today's weather.

---

## In APSIM

### Purpose

1. **Input data source**: Read daily weather records from `.met` file
2. **Data supply**: Provide each component with daily values
3. **Validation**: Check that dates align with Clock
4. **Integration**: Drive water balance (rainfall, evaporation) and crop phenology (temperature, radiation)

### Configuration

| Parameter | Type | Typical Value | Notes |
|-----------|------|---------------|-------|
| **FileName** | Path | Dalby.met | Must exist in simulation directory or be absolute path |
| **Latitude** | Decimal | -27.18 | For calculating potential evaporation (PET) |
| **Longitude** | Decimal | 151.26 | For future use in spatial analyses |

### Daily Variables Supplied

| Variable | Units | Used By | Notes |
|----------|-------|---------|-------|
| **MaxT** | °C | Thermal time, PET, crop stress | Daily maximum temperature |
| **MinT** | °C | Thermal time, frost check, PET | Daily minimum temperature |
| **Rainfall** | mm | ESW update, runoff | Daily precipitation |
| **Radiation** | MJ/m²/day | Photosynthesis, PET | Solar radiation (or sunshine hours) |
| **RelativeHumidity** | % | PET, dew formation | Optional; calculated if not supplied |
| **WindSpeed** | m/s | PET, evaporation | Optional; default 2 m/s if not supplied |

---

## Key Concepts

### Weather File Format (`.met`)

**SILO/BOM standard header**:
```
[weather.met]
!Weather file for Dalby, QLD, Australia
Latitude = -27.18
Longitude = 151.26
site name = Dalby
[weather_header]
! Date_str Year Month Day Rain Maxt Mint Radn
[environment]
1950 1 1 0.0 20.1 8.3 18.5
1950 1 2 5.2 21.3 9.1 19.2
1950 1 3 0.0 22.5 10.1 20.1
...
```

### Potential Evapotranspiration (PET)

**Definition**: Maximum water loss from soil + vegetation under non-limiting conditions.

**Formula** (Penman-Monteith):
```
PET = f(T_max, T_min, Radiation, Humidity, Wind, Latitude)
```

**Role in ESW**: PET drives daily ESW decline (Actual ET when water is available).

### Actual Evapotranspiration (AET)

**Definition**: Water loss when soil is water-stressed (ESW low).

```
AET = PET × StressFactor
where StressFactor = f(ESW/ESW_critical)
```

- If ESW > ESW_critical → AET ≈ PET
- If ESW < ESW_critical → AET < PET (crop water-stressed)

---

## Regional Weather Patterns

### Mediterranean Climate (e.g., Dalby, QLD)

- **Summer** (Nov–Mar): Low rainfall, high T, high radiation → ESW declines
- **Winter** (Jun–Aug): High rainfall (frontal systems), low T → ESW replenished
- **Shoulder seasons** (Apr–May, Sep–Oct): Variable; sowing windows typically occur

**Implication**: Sowing decisions (ESW ≥ 100 mm + rain ≥ 25 mm) typically align with autumn break (late Mar–Apr) when rainfall increases.

### Subtropical Climate (e.g., inland Queensland)

- **Spring** (Sep–Nov): Unpredictable; some years dry, others wet
- **Summer** (Dec–Feb): High rainfall, high T; crop peak growth but risk of heat stress
- **Autumn/Winter** (Mar–Aug): Declining rainfall; cooler T; crop matures

**Implication**: Sowing date experiments (Exercise C) explore trade-offs between early (more season length) vs. late (skip spring drought) sowing.

---

## Common Misconceptions

### 1. "I can use daily-averaged or hourly-interpolated weather data"
**Clarification**: APSIM requires daily max, min, and total radiation (not hourly averages). Hourly data must be aggregated to daily; daily averages won't work (need T_max, T_min, not T_mean).

### 2. "Weather stations far away are fine; climate is the same regionally"
**Clarification**: Weather varies significantly over 50–100 km, especially rainfall. Use local station data when possible. Regional models (e.g., SILO) interpolate between stations and are preferred.

### 3. "Missing weather data can be interpolated or skipped"
**Clarification**: APSIM will crash if Clock spans a date not in the weather file. Use gap-filling methods (interpolation, neighboring station) to complete the record before using in APSIM.

### 4. "Changing weather file won't affect ESW or crop yield much"
**Clarification**: Weather is the primary driver. Different rainfall patterns (even same annual total) can change yield by 30–50% or more.

---

## Troubleshooting

### Weather file not found

**Checklist**:
1. Is the `.met` file in the same folder as the `.apsimx` file?
2. If not, is the path in Weather component absolute and correct?
3. Check for typos in filename (e.g., `Dalby.met` vs. `dalby.met` — case sensitive on Linux)

**Diagnostic**: Try copying the `.met` file to the same folder as `.apsimx` and use relative path `Dalby.met`.

### Weather data ends before Clock end date

**Example**:
- Clock end date: 1960-12-31
- Weather file end date: 1960-06-30
- Result: Simulation crashes on 1960-07-01 ✗

**Fix**: Either truncate Clock or extend weather data (gap-fill).

### Unrealistic PET or AET values

**Checklist**:
1. Are temperature units correct? (Must be °C, not K or F)
2. Are radiation units correct? (Must be MJ/m²/day, not kWh or langleys)
3. Are latitude/longitude in correct range? (Latitude -90 to +90, Longitude -180 to +180)

**Diagnostic**: Check daily PET against regional literature (e.g., Australia PET ~3–7 mm/day typical; > 10 mm/day suggests data error).

---

## Exercise References

- **Module 2** (First Simulation): Load weather data; understand PET concept
- **Module 3** (Reading Output): Analyze ESW vs. rainfall patterns; identify water-stressed periods
- **Exercise A** (Fallow): 7-year water balance; see how rainfall/PET interact
- **Exercise D** (20-Year Variability): Compare yields across different rainfall years

---

## Related Concepts

- **[[Clock]]** — Must span weather file date range
- **[[Water_Balance]]** — PET drives ESW decline; rainfall increases ESW
- **[[Thermal_Time]]** — T_max and T_min drive GDD accumulation
- **[[ESW_Extractable_Soil_Water]]** — AET calculated from PET and ESW

---

## Resources

- **SILO data**: https://www.longpaddock.qld.gov.au/silo/ (1889–present, interpolated)
- **BOM**: http://www.bom.gov.au/ (station-based, quality-controlled)
- **APSIM format**: https://apsimnextgeneration.netlify.app/ → Weather file format
- **Example**: `wheat_example.apsimx` includes `Dalby.met` in download
