---
title: "Entity: Thermal Time (GDD)"
tags: [entity, phenology, module-5, module-6]
related: [[Wheat]], [[Zadok_Scale]], [[Phenology]], [[Phenology_Thermal_Time_Link]]
last_updated: 2026-05-01
status: draft
---

# Thermal Time (Growing Degree Days)

## Definition

**Thermal Time** or **Growing Degree Days (GDD)** is the cumulative sum of daily temperature above a base threshold:

$$
\text{GDD} = \sum_{t=0}^{n} \max(0, T_{\text{avg}} - T_{\text{base}})
$$

where:
- $T_{\text{avg}}$ = daily average temperature = (T_max + T_min) / 2
- $T_{\text{base}}$ = base temperature (varies by crop; wheat ≈ 0°C pre-vernalization, 3–4°C post-vernalization)
- n = number of days

**Example**: If T_avg = 20°C and T_base = 0°C, then GDD for that day = 20. Over 50 days at T_avg = 20°C, cumulative GDD = 1000.

## Why Thermal Time Matters

Crops don't develop based on calendar days; they develop based on accumulated heat. 

- **Calendar days**: Unreliable across climates and years
  - Wheat might take 120 days to mature in cool Tasmania
  - Same wheat might take 90 days in warm Queensland
  
- **Thermal time**: Reliable across climates and years
  - Wheat reaches physiological maturity at ~1100–1200 GDD regardless of location
  - This allows crop models to predict phenology consistently

## Key Parameters for Wheat

| Parameter | Value | Notes |
|-----------|-------|-------|
| **T_base** (pre-vernalization) | 0°C | Before winter cold period |
| **T_base** (post-vernalization) | 3–4°C | After winter cold period |
| **T_opt** (optimal) | ~20°C | Peak development rate |
| **GDD to emergence** | 50–80 | From sowing to first leaf |
| **GDD to anthesis** | 850–950 | From sowing to flowering |
| **GDD to maturity** | 1100–1300 | From sowing to physiological maturity (cultivar-dependent) |

## Role in APSIM

APSIM tracks thermal time for each crop:

1. **Daily accumulation**: Each day, GDD += max(0, T_avg − T_base)
2. **Stage advancement**: When GDD crosses thresholds, phenological stage changes
   - GDD = 0–80: Emergence
   - GDD = 80–600: Vegetative growth
   - GDD = 600–850: Reproductive (booting to heading)
   - GDD = 850–1100: Grain fill
   - GDD > 1100: Maturity
3. **Yield prediction**: Grain fill depends on GDD duration and crop N/water status

## Effects of Temperature Stress

### Heat Stress (T > T_opt)
- Thermal time **accumulates faster** (warmer = higher T_avg − T_base)
- **Result**: Crop matures earlier but may have less time for grain fill
- **Example**: A 25°C day vs 20°C day accumulates 25 vs 20 GDD (25% more)

### Cold Stress (T < T_base)
- Thermal time **doesn't accumulate** (max(0, T_avg − T_base) = 0)
- **Result**: Development pauses; extended timeline
- **Example**: A 2°C day accumulates 0 GDD (assuming T_base = 0°C)

### Vernalization (Winter Wheat)
- Winter wheat requires **cold period** (20–30 days < 10°C) to induce flowering
- Only after vernalization does T_base shift to 3–4°C and anthesis becomes possible
- **APSIM**: Tracks vernalization state; won't flower until requirement met

## Exercises Where Thermal Time is Central

- [[Module_5_Phenology_Guide]]: Understand GDD accumulation and phenological stages
- [[Exercise_C_Sowing_Date_Phenology_Guide]]: Different sowing dates experience different temperatures → different GDD progressions → different frost/heat risk windows

## Common Student Misconceptions

### Misconception 1: "Thermal time is the same everywhere"
**Clarification**: No. Thermal time accumulates **daily** based on local temperature. Cool location (e.g., Tasmania) accumulates fewer GDD/day than warm location (e.g., Queensland).

### Misconception 2: "Hotter temperature = always better for crop"
**Clarification**: Hotter speeds development (more GDD/day) but may shorten grain-fill duration or increase water stress. Sweet spot is ~20°C; much hotter or colder is suboptimal.

### Misconception 3: "A 25°C day counts as 25 GDD"
**Clarification**: True if T_base = 0°C. But if T_base = 3°C, then a 25°C day = 22 GDD. T_base varies by crop and growth stage.

### Misconception 4: "Thermal time predicts everything"
**Clarification**: Thermal time predicts **phenology** (stage), not **yield**. Yield also depends on N, water, light, management.

## Troubleshooting

### Issue: Phenology stuck; thermal time not advancing
- **Check**: Temperature in weather file? (Is simulation in winter when T_base barely exceeded?)
- **Check**: Frost event? (If T < T_base, GDD doesn't accumulate)
- **Check**: Vernalization met? (Winter wheat won't flower until cold requirement satisfied)

### Issue: Wheat matures too early or too late
- **Check**: Cultivar choice? (Different cultivars have different GDD requirements)
- **Check**: Thermal time calculation? (Is T_base correct for growth stage?)

## Sources & References

- [[Holzworth_2014_APSIM_Summary]]: Phenology and thermal time implementation
- [[APSIM_Internal_Guide.md]]: Crop development model details
- [Zadok Scale](https://en.wikipedia.org/wiki/Feekes_scale): Phenological stage classification

---

**Last Updated**: May 1, 2026  
**Inbound Links**: [[Wheat]], [[Zadok_Scale]], [[Exercise_C_Sowing_Date_Phenology_Guide]]  
**Status**: Draft
