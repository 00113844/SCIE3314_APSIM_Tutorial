---
title: Zadok_Scale
tags: [entity, reference-scale, phenology]
related: [[Thermal_Time]], [[Wheat]], [[Phenology_Link]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Zadok Scale — Standardized Wheat Phenological Stages

## Definition

The **Zadok scale** (0–99) is a standardized system for describing wheat phenological development from sowing (0) through ripeness (90+). Each stage corresponds to a visible morphological milestone (leaf appearance, stem elongation, head emergence, grain development). APSIM tracks Zadok stage as a decimal (0.0–90.9) to allow fractional progress between major stages.

**Key role**: Zadok scale provides a common language for describing crop development; critical for understanding when to apply fertilizer, when to expect harvest, when water/N stress matters most.

---

## In APSIM

### Zadok Stage Mapping to GDD

APSIM calculates thermal time (GDD) and maps accumulated GDD to Zadok stages:

| Stage | Description | GDD Range (Typical) | Days After Sowing |
|-------|---|---|---|
| **0–9** | Germination / Imbibition | 0–50 | 0–10 |
| **10–19** | Emergence (seedling leaves) | 50–100 | 10–20 |
| **20–29** | Leaf development (tillering) | 100–300 | 20–60 |
| **30–39** | Stem elongation (growing point differentiated) | 300–600 | 60–120 |
| **40–49** | Booting (flag leaf / boot stage) | 600–750 | 120–150 |
| **50–59** | Heading / Anthesis (head emerges to flowering) | 750–900 | 150–180 |
| **60–69** | Anthesis / Pollination | 850–950 | 170–195 |
| **70–79** | Grain development (milk to soft dough) | 950–1100 | 195–240 |
| **80–89** | Grain ripening (hard dough to hard grain) | 1100–1200 | 240–280 |
| **90+** | Maturity / Ripeness (harvest-ready) | 1200–1300+ | 280–310+ |

---

## Key Zadok Stages (Pedagogic Importance)

### Emergence (Stage 10)

**Definition**: Seedling leaves visible above soil surface  
**GDD**: ~50 from sowing  
**Timing**: 1–2 weeks after sowing  
**Why critical**: 
- Marks successful crop establishment
- If never reaches Stage 10, sowing failed (frost, dry conditions, deep planting)
- Exercise A exercises focus on emergence success

### Booting (Stage 45)

**Definition**: Flag leaf visible; stem rigid; head still in boot (inside leaf sheath)  
**GDD**: ~600–700  
**Timing**: ~3 months from sowing  
**Why critical**:
- Last major vegetative stage; N uptake nearly complete
- Drought/N stress applied here has severe impact on yield
- Ideal time for late N application (if not applied earlier)

### Anthesis / Heading (Stage 55–60)

**Definition**: Head emerges; pollen shed begins  
**GDD**: ~800–900  
**Timing**: ~4 months from sowing  
**Why critical**:
- Determines final grain number (florets that will become grain)
- Critical period for water/N stress (highest sensitivity)
- Observable in field (head emergence is visible checkpoint)

### Grain Fill / Milk Stage (Stage 70–75)

**Definition**: Grains are soft ("milky" if squeezed); rapidly accumulating dry matter  
**GDD**: ~950–1050  
**Timing**: ~5 months from sowing  
**Why critical**:
- Grains growing fastest; highest transpiration demand
- Drought stress here reduces final grain weight 30–50%
- Typically 3–4 weeks of peak demand

### Physiological Maturity (Stage 90–92)

**Definition**: Grain no longer filling; black layer visible at base of grain; maximum dry mass achieved  
**GDD**: ~1100–1300  
**Timing**: ~6–7 months from sowing  
**Why critical**:
- Marks end of grain development; yield is now fixed
- Harvest window opens; grain can be safely collected
- After this, further rain/warm weather causes sprouting, not more growth

---

## Typical Timeline (Mediterranean Climate, Dalby-like)

```
March 20      Sowing (ESW > 100 mm, rain > 25 mm)
             ↓ (Stage 0)

April 5       Emergence (Stage 10)
             ↓

May 1         Tiller initiation (Stage 20–25)
             ↓

June 15       Stem elongation begins (Stage 30)
             ↓

July 20       Booting, flag leaf visible (Stage 45)
             ↓

August 15     Head emergence, anthesis begins (Stage 55–60)
             ↓ [CRITICAL: N stress, water stress here → yield loss]

September 20  Grain fill (milk stage, Stage 70–75)
             ↓ [HIGH transpiration demand]

October 30    Physiological maturity (Stage 90)
             ↓

November 10   Harvest-ready (Stage 92–93; grain hard, moisture < 14%)
```

---

## Common Misconceptions

### 1. "Zadok stage is the same as calendar days"
**Clarification**: Zadok stage is driven by thermal time (GDD), not calendar days. Warm year = faster progression; cool year = slower progression. Two dates differing by 10 calendar days might both be Zadok Stage 50, or one might be Stage 45 and other Stage 55, depending on temperature accumulation.

### 2. "All wheat reaches Zadok Stage 90 (maturity) at same age"
**Clarification**: GDD accumulation varies by year and cultivar. In cool year, stage 90 might occur at DAS 300; in warm year, DAS 270. Exercise D explores this variability.

### 3. "Zadok stage is just for field observation; not important for APSIM simulation"
**Clarification**: Zadok stage (or GDD-equivalent) drives key simulation decisions:
- Phenology (when to transition stages)
- Organ development (leaf appearance, tiller number, grain number, root depth)
- Manager rules (harvest at Stage 90)
- Stress response (N, water stress impact varies by stage)

### 4. "Between two Zadok integers (e.g., 55.3), the crop is undefined"
**Clarification**: APSIM tracks continuous Zadok score (e.g., 55.3 = 30% of the way from Stage 55 to Stage 56). This allows smooth progress; crop properties (LAI, root depth, grain size) interpolate smoothly, not in jumps.

---

## Stage-Specific Management

### N Application Timing

- **Early Season (Stage 10–30)**: Modest N uptake; can wait for booting application
- **Booting (Stage 45)**: Last opportunity for N application; uptake peaks in next 4 weeks
- **Post-Anthesis (Stage 65+)**: Too late; further N won't contribute to grain

**Exercise B explores**: Does timing of 160 kg/ha N matter? (all on Sept 1 vs. split applications)

### Irrigation Timing (if applicable)

- **Stage 45–55** (booting to anthesis): Highest priority; water stress now → grain number loss
- **Stage 70–75** (grain fill): Second priority; reduces grain weight if water-stressed
- **Stage 90+**: Low priority; crop near maturity anyway

### Harvest Timing

- **Before Stage 90**: Grain still soft; will not thresh cleanly; yield loss
- **Stage 90–92**: Optimal harvest window; grain hard; maximum yield
- **After Stage 92**: Grain over-mature; can shatter (fall from head); risk if rain/wind occurs

---

## Troubleshooting

### Crop reaches Stage 50 but never progresses further

**Checklist**:
1. Is temperature sufficient? (If very cool year, GDD accumulation slow; check thermal time)
2. Is the crop experiencing severe drought? (Water stress can halt phenological development)
3. Is the crop N-deficient? (Extreme N deficiency can delay development)
4. Did simulation end before crop matured? (Check Clock end date)

**Diagnostic**: Plot [Wheat].Stage daily; should show continuous increase, not plateaus. Plateaus suggest external stress.

### Zadok stage jumps or is erratic

**Checklist**:
1. Check if frost, heatwave, or extreme rainfall occurred on that date (can slow/halt GDD)
2. Verify weather data quality (temperature spikes suggest data error)
3. Check Manager rule — did crop get harvested/reset? (Would reset stage to 0)

### Maturity arrives too early or too late

**Checklist**:
1. Is T_base parameter correct for cultivar? (Winter wheat T_base = 0–3°C; spring wheat = 4–5°C)
2. Is GDD total expected? (Wheat typically 1100–1300 GDD to maturity; if > 1400, something wrong)
3. Did cultivar assumptions match reality? (Different varieties have different GDD requirements)

---

## Exercise References

- **Module 1** (Structure): Understand Zadok scale vs. calendar days
- **Module 5** (Phenology): Track Zadok stages across season; identify N-critical period
- **Exercise B** (N Response): N application at booting (Stage 45) vs. earlier; compare yields
- **Exercise C** (Sowing Date): Different sowing dates → different calendar dates for each Zadok stage; same GDD timing
- **Exercise D** (20-Year Variability): Stage timing varies across years; yield implications

---

## Related Concepts

- **[[Thermal_Time]]** — GDD drives Zadok stage progression
- **[[Wheat]]** — Crop model internally tracks Zadok stage
- **[[Manager]]** — Harvest rule often uses "Stage == 90" criterion
- **[[Phenology_Link]]** — Conceptual connection between thermal time and field-observable stages

---

## Resources

- **Zadok et al. (1974)**: Original paper defining the scale (classic reference)
- **APSIM Wheat Phenology**: https://apsimnextgeneration.netlify.app/ → Wheat model documentation
- **Field observer's guide**: GRDC (Grain Research & Development Corporation) publications on growth stage identification
- **Example**: `wheat_example.apsimx` output includes daily [Wheat].Stage column
