---
title: Soil_Layers
tags: [entity, soil-structure, water-properties]
related: [[Soil]], [[Water_Balance]], [[ESW_Extractable_Soil_Water]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Soil Layers — Multi-Layer Profile Structure

## Definition

**Soil layers** represent horizontal divisions of the soil profile, each with distinct physical and chemical properties. A typical APSIM simulation uses 7 layers (0–1500+ mm depth), allowing detailed tracking of water movement, nitrogen cycling, and root growth across the soil profile.

**Key role**: Layering captures soil heterogeneity (texture changes, drainage rate variations, nutrient availability at depth), which significantly affects crop available water, N leaching, and root development.

---

## In APSIM

### Why Layers?

1. **Texture varies with depth**: Topsoil (loamy) ≠ subsoil (clay); different water-holding capacities
2. **Drainage rate changes**: Sandy layer (fast) vs. clay layer (slow) affects ESW dynamics
3. **Root extraction heterogeneous**: Roots preferentially take water from upper layers (less effort); deeper layers contribute 20–30% of seasonal uptake
4. **N leaching stratified**: Nitrate (mobile) leaches through upper layers, accumulates deeper
5. **Temperature effects**: Surface temperature varies more than subsoil; affects mineralization, nitrification rates

### Typical 7-Layer Profile

```
Layer  | Depth (mm) | Thickness (mm) | Soil Type      | LL    | DUL   | SAT   | KS
-------|------------|---------------|----------------|-------|-------|-------|------
1      | 0–100     | 100           | Loam (topsoil) | 0.20  | 0.40  | 0.50  | 50
2      | 100–250   | 150           | Loam           | 0.20  | 0.40  | 0.50  | 40
3      | 250–450   | 200           | Clay loam      | 0.22  | 0.38  | 0.48  | 20
4      | 450–750   | 300           | Clay           | 0.25  | 0.35  | 0.45  | 10
5      | 750–1100  | 350           | Clay           | 0.25  | 0.34  | 0.44  | 8
6      | 1100–1500 | 400           | Clay           | 0.25  | 0.33  | 0.43  | 5
7      | 1500+     | 500           | Clay (subsoil) | 0.26  | 0.32  | 0.42  | 3
```

**Key observations**:
- LL/DUL decrease with depth (clay has less pore space, more bound water)
- KS decreases sharply with depth (clay is much tighter; drainage slower)
- Thickness increases downward (finer resolution near roots, coarser deeper)

---

## Layer-Specific Processes

### Water Movement

**Each layer updates daily**:
```
ESW(Layer i, today) = ESW(Layer i, yesterday) 
                    + Rainfall_infiltrated(i) 
                    - Runoff_from_layer(i)
                    - Drainage_to_layer_below(i)
                    - Evaporation_from_layer(i)
                    - Plant_uptake(i)
```

**Drainage rate** (mm/day) depends on KS:
- Layer 1 (KS=50): Drains at 50 mm/day if saturated
- Layer 4 (KS=10): Drains at 10 mm/day (5× slower)
- Layer 7 (KS=3): Drains at 3 mm/day (17× slower)

**Implication**: Deep layers retain water much longer; can sustain crop in drought.

### Root Extraction

**Root depth by crop stage**:
- Emergence (Stage 10): Roots to 100 mm (Layer 1 only)
- Booting (Stage 45): Roots to 600–800 mm (Layers 1–4)
- Heading (Stage 55): Roots to 1000–1200 mm (Layers 1–6)
- Maturity (Stage 90): Roots to 1200–1500 mm (all layers)

**Root distribution** (preferential uptake from upper layers):
```
% uptake by layer (typical): 
Layer 1: 35% of total seasonal uptake
Layer 2: 25%
Layer 3: 15%
Layer 4: 10%
Layers 5–7: 15% combined
```

**Why upper layers dominate?** Lower energy cost; less resistance to water flow in upper soil.

### Nitrogen Transformations

**Mineralization** (organic N → NH₄):
- Layer 1 (warm, aerated): 0.05% organic pool/day
- Layer 4 (cooler, waterlogged): 0.01% organic pool/day (10× slower)

**Nitrification** (NH₄ → NO₃):
- Layer 1–3 (aerobic): 20–30% NH₄ pool/day
- Layer 4–7 (anaerobic when saturated): 0–5% NH₄ pool/day

**Leaching** (NO₃ moves with drainage):
- High KS (Layer 1): NO₃ can leach rapidly
- Low KS (Layer 7): NO₃ accumulates; minimal leaching

**Implication**: Deep layers can accumulate residual N from previous years; can be accessed in drought.

---

## Layer Configuration Best Practices

### Thin Upper Layers (0–500 mm)

**Why**: Root zone changes rapidly (daily updates needed)
- Layer 1: 100 mm
- Layer 2: 150 mm
- Layer 3: 200 mm

### Thicker Lower Layers (500+ mm)

**Why**: Changes slower; coarser resolution acceptable
- Layers 4–5: 300–350 mm each
- Layers 6–7: 400–500 mm each

### Minimum Depth

- **Shallow-rooting crops** (lettuce): 300–400 mm
- **Medium-rooting crops** (wheat, maize): 1200–1500 mm
- **Deep-rooting crops** (lucerne): 1800–2400 mm

### Insufficient Depth

If profile is too shallow, crop cannot access deep water → **severe yield underestimate**.

**Example**:
- Actual wheat roots to 1200 mm; can access 150 mm deep ESW
- APSIM profile only 600 mm deep; can access 60 mm ESW
- Result: 2.5× overestimate of water stress; underestimate of yield by 20–30%

---

## Layer Properties: Interpretation

### Lower Limit (LL)

**Definition**: Water content at wilting point (crop cannot extract below this)

**Typical ranges**:
- Sandy soil: 0.10–0.15 mm/mm (low)
- Loamy soil: 0.15–0.25 mm/mm (medium)
- Clay soil: 0.25–0.35 mm/mm (high; much water is bound)

**Why clay is high**: Clay particles are small; have large surface area; bind water molecules via electrostatic forces.

### Drained Upper Limit (DUL)

**Definition**: Water content after 2–3 days of drainage (field capacity)

**Typical ranges**:
- Sandy soil: 0.20–0.30 mm/mm (low)
- Loamy soil: 0.35–0.45 mm/mm (medium)
- Clay soil: 0.30–0.40 mm/mm (medium; paradoxically lower than some loams because macropore drainage)

### Saturation (SAT)

**Definition**: All pores filled; O₂ excluded (anaerobic)

**Typical ranges**:
- All textures: 0.40–0.60 mm/mm (pore space; depends on packing density)

**Interpretation**: SAT – DUL = water-logged range (anaerobic); typically 0.05–0.15 mm/mm.

---

## Common Misconceptions

### 1. "All layers have the same water characteristics; use one template"
**Clarification**: Soil is heterogeneous. Each layer often has different texture, LL, DUL, KS. Using the same values everywhere will produce incorrect simulation. Always check soil survey or APSOIL database for realistic profiles.

### 2. "Deep layers don't matter; only top 500 mm has roots"
**Clarification**: Wheat roots to 1200–1500 mm. Deep layers provide 30–50% of seasonal water. Ignoring them significantly underestimates available water and overestimates drought stress.

### 3. "Drainage rate (KS) is the same as infiltration rate"
**Clarification**: 
- **Infiltration** = water entering soil from surface (driven by rainfall)
- **Drainage** = water moving downward within soil (driven by gravity, matric potential)
- KS is drainage rate, not infiltration rate; they're different processes.

### 4. "If I use only 1–2 layers, it's 'simpler' and should work fine"
**Clarification**: Fewer layers = loss of information. Simulations become inaccurate:
- Cannot capture stratified N leaching
- Cannot represent different drainage rates at depth
- Root extraction incorrectly simplified
- Water balance distorted

**Result**: Biased yield predictions; unreliable for management decisions.

---

## Troubleshooting

### Crop is water-stressed despite high rainfall

**Checklist**:
1. Is profile deep enough? (Wheat needs 1200–1500 mm; if only 600 mm configured, artificial drought)
2. Are DUL/LL values realistic? (Check soil survey; DUL too low → less available water)
3. Is KS too high? (Water drains too fast before crop can uptake)
4. Are root depths growing? (Check that roots reach deeper layers as crop develops)

**Diagnostic**: Export daily [Soil].SoilWater.ESW by layer; if upper layers dry but lower layers full, roots may not be accessing deep water (check root depth).

### Crop is never water-stressed (unrealistic)

**Checklist**:
1. Is profile too thick? (More water available than realistic)
2. Are DUL values too high? (Holding more water than real soil)
3. Is KS too low? (Water retained too long; not draining away)

### N leaching is too high or too low

**Checklist**:
1. Are KS values correct? (Too high = fast leaching; too low = accumulation)
2. Are soil organic N and nitrification rates realistic? (Check against soil survey)
3. Is drainage volume realistic? (High drainage → high leaching; correlate to rainfall, infiltration)

---

## Exercise References

- **Module 2** (First Simulation): Understand layer structure; visualize water distribution
- **Exercise A** (Fallow): 7-year water balance; see how layers interact with rainfall patterns
- **Exercise B** (N Response): Monitor NO₃ by layer; see N leaching pattern
- **Exercise D** (20-Year Variability): In dry years, deep layers sustain crop; in wet years, upper layers waterlog

---

## Related Concepts

- **[[Soil]]** — Profile structure; contains layers
- **[[Water_Balance]]** — Daily update occurs in each layer
- **[[ESW_Extractable_Soil_Water]]** — Sum of ESW across layers with roots
- **[[Nitrogen_Uptake]]** — Root distribution across layers drives uptake

---

## Resources

- **APSOIL database**: https://www.apsim.info/ (download realistic soil profiles)
- **Australian Soil Resource Information System (ASRIS)**: https://www.asris.csiro.au/
- **Soil survey reports**: State agricultural departments (e.g., Queensland DPI) publish soil profiles by region
- **Soil physics**: Brady, N. C., & Weil, R. R. (2017). *The Nature and Properties of Soils* (15th ed.). Chapters on water retention, drainage.
