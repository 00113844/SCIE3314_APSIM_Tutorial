---
title: Manager
tags: [entity, core-component, control-logic]
related: [[Clock]], [[Soil]], [[Wheat]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Manager — Simulation Control Logic

## Definition

The **Manager** component implements the human decision-making layer of the simulation. It reads current simulation state (date, ESW, phenological stage) and executes actions (sow crop, apply fertilizer, harvest) using if-then rules (JavaScript or GUI-based).

**Key role**: Manager bridges the gap between automated soil/crop processes and human management practices (planting, fertilization, harvest timing).

---

## In APSIM

### Purpose

1. **Decision logic**: Evaluate conditions each day
2. **Action execution**: Trigger sowing, fertilizer, harvest, or other events
3. **Parameterization**: Encode "what would a farmer do?" into rules
4. **Flexibility**: Support different management strategies (irrigation, multiple crops, rotations)

### Configuration

**Two approaches**:
1. **GUI-based**: Visual rules in APSIM interface (simpler; limited flexibility)
2. **JavaScript**: Custom code in Manager node (powerful; full control)

---

## Typical Rules

### Sowing Rule

```javascript
// Trigger sowing when soil is moist AND has received spring rain

if (esw > 100 && 
    rain_last_7_days > 25 && 
    today.date >= "01-Mar" && 
    today.date <= "30-Jun" &&
    crop.stage == "not_planted")
{
    sow(crop="Wheat", density=120, depth=30);
}
```

**Translation**: "If ESW exceeds 100 mm AND it rained at least 25 mm in the last 7 days AND we're between March 1 and June 30 AND no crop is currently planted, then sow wheat."

**Rationale**: 
- ESW > 100 mm ensures soil moisture for emergence
- Rain > 25 mm in 7 days signals "break of season"
- Date window matches typical Australian autumn break
- Prevents multiple sowings in same season

### Fertilizer Rule

```javascript
// Apply 160 kg/ha NO₃-N on Sept 1 (pre-season)

if (today.date == "01-Sep" && crop.stage != "not_planted") {
    apply(fertiliser_type="UreaAmmoniumNitrate", amount=160, depth=50);
}
```

**Translation**: "On September 1st, if a crop is planted, apply 160 kg/ha of UAN fertilizer."

**Rationale**:
- Sept 1 = early spring, optimal for N uptake (crop has 4+ months to grow)
- UAN = nitrate form; immediately available
- 160 kg/ha = typical N rate for wheat in Australia

### Harvest Rule

```javascript
// Harvest when crop reaches physiological maturity

if (wheat.stage == "PhysiologicalMaturity" || 
    wheat.stage == "Ripe") {
    harvest(crop="Wheat", remove="GrainAndStraw");
}
```

**Translation**: "When the crop reaches physiological maturity (or ripe stage), harvest grain and straw."

**Rationale**:
- Physiological maturity = maximum grain mass; further waiting = grain loss
- Removes both grain (for yield) and straw (residue for next rotation)

---

## Key Concepts

### Daily Evaluation

Manager rules are evaluated **once per day**, in sequence, after Weather but before Soil/Crop processes. This means:

- Today's Weather data is available for decision-making
- Rules fire in order (first sowing rule, then fertilizer rule, then harvest)
- Action takes effect immediately; Crop/Soil respond same day

### JavaScript Syntax (if using code-based rules)

**Available variables**:
- `today.date` — Current date (formatted as "DD-Mon" or "YYYY-MM-DD")
- `crop.stage` — Current phenological stage ("not_planted", "Germinating", "Emerged", etc.)
- `esw` — Extractable soil water (mm) from Soil component
- `rain_last_7_days` — Sum of rainfall over last 7 days (mm)
- `wheat.biomass` — Current crop biomass (kg/ha)

**Actions available**:
- `sow(crop="Wheat", density=..., depth=...)` — Plant crop
- `apply(fertiliser_type="...", amount=...)` — Add fertilizer
- `harvest(crop="Wheat", remove="...")` — Remove crop
- `irrigate(amount=...)` — Apply irrigation water

---

## Regional Variations

### Mediterranean Climate (e.g., Dalby)

- **Sowing window**: Late March – late April (autumn break)
- **Fertilizer timing**: Early September (spring)
- **Harvest window**: November – December (before summer heat)
- **Rule adjustment**: ESW > 100 mm + rain > 25 mm typical; may need to relax in dry years

### Subtropical Climate (inland Queensland)

- **Sowing window**: Wider (May–June preferred, but can sow July–Aug)
- **Fertilizer timing**: Early to mid-September (earlier spring start)
- **Harvest window**: October – November (cooler than Mediterranean)
- **Rule adjustment**: Often require ESW > 150 mm to reduce risk (sowing into drier conditions more risky)

---

## Common Misconceptions

### 1. "Manager rules fire on every simulation step, not just daily"
**Clarification**: Manager is evaluated exactly once per day, synchronized with Clock. There is no sub-daily rule evaluation.

### 2. "All rules fire simultaneously; order doesn't matter"
**Clarification**: Rules fire in sequence (top to bottom). If Rule 1 harvests, Rule 3 can't sow the same day. Order matters!

### 3. "I can use simple dates like 'spring' instead of specific dates"
**Clarification**: Dates must be specific (e.g., "01-Sep", not "spring"). Some rules use date ranges (if-then checks `date >= ... && date <= ...`).

### 4. "Sowing rule triggers when ESW first exceeds 100 mm"
**Clarification**: Sowing rule checks ESW **and** recent rain. If ESW > 100 but no rain in 7 days, no sowing. Both conditions must be true.

---

## Troubleshooting

### Crop never sows

**Checklist**:
1. Do sowing conditions ever occur? (Check ESW trajectory; is it ever > 100 mm?)
2. Does rainfall pattern match rule? (Is there ever 25+ mm in a 7-day window?)
3. Is sowing date within the allowed window? (Check date range in rule)
4. Is the crop already planted? (Rule requires `stage == "not_planted"`)

**Diagnostic**: Add debug output (if using JavaScript):
```javascript
if (today.date.month == 4) { // Print ESW every day in April
    print("Day: " + today.date + ", ESW: " + esw + ", Rain7d: " + rain_last_7_days);
}
```

### Fertilizer doesn't get applied

**Checklist**:
1. Is the date in the rule correct? (Check calendar; Sept 1 is day 244 of year)
2. Is a crop already planted on that date? (Rule requires `crop.stage != "not_planted"`)
3. Is fertilizer amount in correct units? (kg/ha or g/m²? Check APSIM convention)

### Harvest never triggers

**Checklist**:
1. Does crop ever reach physiological maturity? (Check thermal time milestones; if GDD too low, may never reach)
2. Is the date window realistic? (e.g., if sowing is Nov, maturity might not occur until next year)
3. Is there a secondary harvest rule preventing it? (Multiple rules can conflict)

---

## Exercise References

- **Module 0** (Why APSIM?): See how Manager rules encode farmer knowledge
- **Module 1** (Structure): Write your first sowing rule
- **Exercise A** (Fallow): No Manager rules; just water balance
- **Exercise B** (N Response): Multiple fertilizer rates in separate Manager rules
- **Exercise C** (Sowing Date): Modify sowing date in Manager; compare outcomes
- **Exercise D** (20-Year Variability): Fixed Manager rules; see how variable rainfall affects outcomes

---

## Related Concepts

- **[[Clock]]** — Provides date for rule evaluation
- **[[ESW_Extractable_Soil_Water]]** — Key variable in sowing decisions
- **[[Wheat]]** — Crop stage drives harvest/N stress decisions
- **[[Water_Balance]]** — Affects ESW threshold in sowing rules

---

## Resources

- **APSIM Documentation**: https://apsimnextgeneration.netlify.app/ → Manager component
- **JavaScript syntax**: https://www.w3schools.com/js/ (if using code-based rules)
- **Example**: `wheat_example.apsimx` includes working sowing/fertilizer/harvest rules
