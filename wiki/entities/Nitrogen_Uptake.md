---
title: Nitrogen_Uptake
tags: [entity, physiological-process, nutrient-cycling]
related: [[Soil]], [[Wheat]], [[AEN_Efficiency]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Nitrogen Uptake — Plant N Demand and Soil Supply

## Definition

**Nitrogen uptake** is the daily movement of inorganic nitrogen (NO₃⁻, NH₄⁺) from soil to plant roots, driven by crop demand (function of growth stage, LAI, available N). APSIM calculates uptake based on crop N demand and soil N availability; N-limited growth results if demand exceeds supply.

**Key role**: N uptake links soil N pools to crop biomass/yield; central to understanding N fertilization (Exercise B, D).

---

## In APSIM

### Purpose

1. **Demand calculation**: Determine N requirement based on growth stage and LAI
2. **Supply constraint**: Check if soil NO₃/NH₄ pools can meet demand
3. **Uptake extraction**: Remove N from soil; add to plant
4. **Stress response**: Reduce growth if N deficient

### Daily Nitrogen Uptake Process

```
Demand = f(LAI, growth_stage, target_grain_N_concentration)
Supply = [NO₃](soil) + [NH₄](soil) [immediately available]
Uptake = min(Demand, Supply)  [take what's available or needed]
Stress_factor = Uptake / Demand  [if 1.0, no stress; if 0.5, 50% constrained]
Crop_growth_multiplier = Stress_factor  [reduce biomass if N-stressed]
```

---

## Key Concepts

### N Demand by Growth Stage

| Stage | LAI | Relative Demand | Timing |
|-------|-----|--|---|
| **Emergence** | 0–1 | Very low (0.5 kg/ha/day) | Days 0–20 |
| **Vegetative** | 1–5 | Moderate (2–3 kg/ha/day) | Days 20–100 |
| **Stem Elongation** | 3–6 | High (3–5 kg/ha/day) | Days 100–150 |
| **Booting/Heading** | 5–7 | High (4–6 kg/ha/day) | Days 150–200 |
| **Grain Fill** | 5–6 (declining) | Moderate–High (2–4 kg/ha/day) | Days 200–280 |
| **Maturity** | 0 (senescing) | Low (0 kg/ha/day) | Days 280+ |

### Soil N Supply

**Available N pools**:

```
Daily Supply = min(
  [NO₃⁻] in rooting zone,
  [NH₄⁺] in rooting zone,
  Demand
)
```

**Limiting factors**:
- If NO₃⁻ is leaching (high drainage) → supply constrained
- If NH₄⁺ hasn't nitrified → slowly available
- If roots haven't reached deep layers → can't access deep N

### Agronomic Efficiency of N (AEN)

**Definition**:
```
AEN = (Yield_at_160 kg/ha_N - Yield_at_0_kg/ha_N) / 160 kg/ha_N

Units: kg grain / kg N applied

Typical range: 10–20 kg grain / kg N
```

**Interpretation**:
- High AEN (20+): N was limiting; response strong
- Low AEN (5–10): N wasn't limiting; diminishing returns
- AEN ≤ 0: "Negative response" (usually measurement error or excessive N)

**Exercise B calculates AEN** across 4 N rates to understand field N responsiveness.

---

## Regional Patterns

### Mediterranean Climate (Dalby-like)

- **Vegetative period** (Mar–Aug): Cool, slow growth, low N demand
- **Grain fill** (Sep–Nov): Warm, rapid growth, high N demand
- **Typical AEN**: 12–16 kg grain/kg N (moderate response)
- **Implication**: Split N application (early Sep + stem elongation) better than single spring dose

### Subtropical Climate (inland QLD)

- **Spring** (Sep–Nov): Variable; some years high demand, others drought-limited
- **Early summer** (Dec–Jan): Peak demand (high LAI, fast growth)
- **Late season** (Feb–Apr): Cooler, declining demand
- **Typical AEN**: 10–18 kg grain/kg N (high variability across years)
- **Implication**: N timing critical; early spring N can be wasted if drought follows

---

## Common Misconceptions

### 1. "More N always means higher yield"
**Clarification**: Beyond optimal N rate, yield plateaus (diminishing returns). Excess N may even reduce yield (lodging, disease, water stress). Exercise B explores this "N response curve."

### 2. "All N fertilizer is immediately available"
**Clarification**: 
- NO₃-N (nitrate) applied: immediately available (same day)
- Urea: requires hydrolysis (1–3 days) before availability
- NH₄-N: requires nitrification (1–7 days) before availability
- Organic N: requires mineralization (weeks to months)

### 3. "Soil organic N supplies all N; fertilizer is extra"
**Clarification**: Organic N mineralizes slowly (0.01–0.05% pool/day). Mineralization + fertilizer together meet crop demand. In high-yield systems, fertilizer is essential; organic N alone is insufficient.

### 4. "N uptake is constant throughout the season"
**Clarification**: N uptake peaks during stem elongation/heading (50–70% total N uptake in 4 weeks). Early and late season uptake much lower. Timing of N application matters greatly.

---

## Troubleshooting

### Crop is N-deficient (pale leaves) despite fertilizer application

**Checklist**:
1. Was fertilizer applied? (Check Manager rule date; was it triggered?)
2. Is applied N amount correct? (e.g., 160 kg/ha, not 160 g/ha?)
3. Is N form correct? (Nitrate is immediately available; urea/organic N delayed)
4. Is soil NO₃ leaching? (High drainage + sandy soil = N loss; check [Soil].NO₃ trajectory)
5. Is crop demanding N? (Check LAI; if still small, demand low)

**Diagnostic**: Export [Soil].NO₃ daily; if it drops to near-zero after fertilizer, N is being leached or rapidly uptaken.

### Yield doesn't increase with N fertilizer (AEN ≈ 0)

**Checklist**:
1. Was the crop water-stressed? (If ESW < 5 mm for weeks, N uptake is blocked)
2. Was the crop already N-sufficient? (If no N deficiency symptoms before fertilizer, N wasn't limiting)
3. Is soil organic N high? (If organic N > 0.3%, natural mineralization may supply enough)
4. Were other factors limiting? (Disease, frost, insufficient light?)

**Diagnostic**: Compare 0 N vs. 160 N yields; if similar, N wasn't the limiting factor.

### Excessive vegetative growth, no grain (all biomass, no seed)

**Checklist**:
1. Is N rate too high? (> 200 kg/ha may cause rank growth, lodging, no grain)
2. Is the crop experiencing drought during grain fill? (Water stress overrides N)
3. Is harvest date correct? (Crop might not have reached maturity yet)

### N in grain is very low despite high soil NO₃

**Checklist**:
1. Is root depth sufficient? (Shallow roots can't extract deep N)
2. Is grain_fill stage too short? (If drought terminates grain fill early, N translocation incomplete)
3. Is there nitrogenation remobilization? (Vegetative N is re-transported to grain during grain fill; check Wheat model parameters)

---

## Exercise References

- **Module 5** (N Fertilisation): Understand N demand across crop stages
- **Exercise B** (N Response): Grow wheat at 0, 80, 160, 240 kg/ha N; calculate AEN
- **Exercise D** (20-Year Variability): Test if AEN varies across years (rainfall effects)

---

## Related Concepts

- **[[Soil]]** — NO₃/NH₄ pools; mineralization, nitrification
- **[[Wheat]]** — LAI and stage drive N demand
- **[[Manager]]** — Applies N fertilizer on schedule
- **[[AEN_Efficiency]]** — Framework for understanding N response

---

## Resources

- **Plant N physiology**: Marschner, H. (2012). *Mineral Nutrition of Higher Plants* (3rd ed.). Academic Press.
- **APSIM N cycling**: https://apsimnextgeneration.netlify.app/ → Nitrogen cycle documentation
- **Field experiment data**: CSIRO APSIM benchmark studies (see APSIM Initiative website)
