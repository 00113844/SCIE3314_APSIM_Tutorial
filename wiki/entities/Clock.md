---
title: Clock
tags: [entity, core-component, simulation-control]
related: [[Manager]], [[Weather]], [[Report]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Clock — Simulation Time Control

## Definition

The **Clock** component defines the temporal boundaries and step size of an APSIM simulation. It controls when the simulation starts, ends, and how time progresses through the timesteps.

**Key role**: Synchronizes all daily processes; ensures all other components operate in sequence on each calendar day.

---

## In APSIM

### Purpose

1. **Initialization**: Set simulation start date
2. **Daily iteration**: Trigger daily timestep sequence
3. **Termination**: Stop simulation at end date
4. **Validation**: Check that all other components (Weather, Soil, Crop) have data for each simulated date

### Configuration

| Parameter | Type | Typical Value | Notes |
|-----------|------|---------------|-------|
| **Start date** | Date | 1950-01-01 | Must align with weather file start date |
| **End date** | Date | 1960-12-31 | Must align with weather file end date |
| **Simulation step** | Integer | 1 (day) | All APSIM simulations use daily timestep |

### Simulation Sequence (per day)

```
Clock: Record date; check if end reached
  ↓
Weather: Read daily record (T_max, T_min, rain, radiation)
  ↓
Managers: Evaluate sowing/fertilizer/harvest rules
  ↓
Soil: Calculate water balance (ESW, drainage, runoff)
  ↓
Crop: Advance phenology, accumulate biomass, take water/N
  ↓
Report: Record outputs to DataStore
  ↓
Next Day
```

---

## Key Concepts

### Daily Timestep Importance

- **Why daily?** Matches weather data resolution (daily measurements)
- **Why not hourly?** Computational cost too high; daily adequate for crop models
- **Why not monthly?** Too coarse; misses critical events (rain, frost, N leaching)

### Clock-Weather Synchronization

**Critical**: Clock start/end dates must fall within the weather file date range.

**Example**:
- Weather file: 1950-01-01 to 1960-12-31
- Clock OK: 1950-01-01 to 1960-12-31 ✅
- Clock FAILS: 1949-01-01 to 1960-12-31 ❌ (start before weather data)
- Clock FAILS: 1950-01-01 to 1961-12-31 ❌ (end after weather data)

---

## Common Misconceptions

### 1. "Clock step can be hourly or monthly"
**Clarification**: All APSIM simulations use daily timestep by design. This is hard-coded in APSIM Next Gen; you cannot change it. The `SimulationStep` parameter is always 1 day.

### 2. "Changing Clock dates doesn't affect other components"
**Clarification**: Clock dates directly control which rows of the weather file are read and which daily calculations occur. Extending Clock beyond weather data will crash the simulation.

### 3. "I can skip days in Clock (e.g., simulate only 1 July – 31 August each year)"
**Clarification**: Clock is continuous. To simulate only certain months, use a fallow period (no crop) or off-season in the Manager rules. You cannot skip dates within the Clock range.

### 4. "Start/end dates don't matter if I have weather data"
**Clarification**: If Clock dates don't match weather file, simulation fails immediately. This is the #1 cause of "Simulation crashed on Day 1" errors.

---

## Troubleshooting

### Simulation crashes immediately

**Checklist**:
1. Does Clock start date = Weather file start date? (Check `.met` file header)
2. Does Clock end date ≤ Weather file end date? (Check `.met` file last row)
3. Are dates in correct format `YYYY-MM-DD`? (e.g., `1950-01-01` not `01/01/1950`)
4. Is the `.apsimx` file using the correct weather file path?

**Diagnostic**: Open the weather `.met` file in a text editor:
```
[weather.met]
!Weather file for Dalby, QLD, Australia
[weather_header]
Latitude = -27.18
site name = Dalby
[environment]
! Date_str Year Month Day Rain Maxt Mint Radn
1950 1 1 0.0 20.1 8.3 18.5
1950 1 2 5.2 21.3 9.1 19.2
...
```
Compare first and last date to your Clock settings.

### Simulation runs but stops early

**Checklist**:
1. Did you intend to stop on that date? (Check `ClockStopDate` in `.apsimx`)
2. Is there a Manager rule that triggers harvest on that date? (Check Manager code)
3. Does weather data have gaps? (Some weather files have missing days)

---

## Exercise References

- **Module 0** (Why APSIM?): Understand why daily timestep
- **Module 1** (Structure): Configure Clock dates matching weather
- **Exercise A** (Fallow): 7-year fallow simulation; Clock spans entire period
- **Exercise C** (Sowing Date): Test Clock over 20-year period; Managers vary sowing rules by year

---

## Related Concepts

- **[[Weather]]** — Data source; must cover Clock date range
- **[[Manager]]** — Rules evaluated daily; triggered by Clock events
- **[[Report]]** — Records outputs each day; one row per Clock day
- **[[Thermal_Time]]** — Accumulates each Clock day; drives phenology

---

## See Also

- CLAUDE.md → "Page Template: Entity" (structure for entity pages)
- APSIM Documentation: https://apsimnextgeneration.netlify.app/ → Clock component
- Example: `wheat_example.apsimx` → Check Clock node for date format
