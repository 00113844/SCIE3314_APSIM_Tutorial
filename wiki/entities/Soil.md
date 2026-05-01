---
title: Soil
tags: [entity, core-component, state-variable]
related: [[Soil_Layers]], [[Water_Balance]], [[Nitrogen_Uptake]], [[ESW_Extractable_Soil_Water]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# Soil — Soil Profile and Properties

## Definition

The **Soil** component represents the physical and chemical characteristics of the soil profile (layered structure, water-holding capacity, nitrogen pools, hydraulic properties). It maintains state variables updated daily (water content, nitrate, ammonium) and calculates soil water balance and nitrogen transformations.

**Key role**: Soil is the reservoir and transformation center for water and nitrogen; all plant uptake is extracted from the soil.

---

## In APSIM

### Purpose

1. **Structure**: Define soil layer thickness, texture, water characteristics
2. **State tracking**: Maintain daily water content (θ) and N pools (NO₃, NH₄, organic N)
3. **Process calculation**: Daily water balance (infiltration, drainage, evaporation) and N cycle (mineralization, nitrification, leaching, uptake)
4. **Crop interaction**: Supply water/N to crop; receive root uptake signals

### Configuration

| Parameter | Type | Unit | Typical Range | Notes |
|-----------|------|------|---------------|-------|
| **Layer 1 Thickness** | mm | - | 100–200 | Top layer; root zone |
| **Layer 1 LL** (Lower Limit) | mm/mm | - | 0.15–0.25 | Water unavailable to plants |
| **Layer 1 DUL** (Drained Upper Limit) | mm/mm | - | 0.35–0.50 | Water after drainage (field capacity) |
| **Layer 1 SAT** (Saturation) | mm/mm | - | 0.45–0.60 | Pore space filled; O₂ depleted |
| **Layer 1 InitialWater** | Fraction | - | 0–1.0 | Initial θ on Day 1 (0.5 = 50% capacity) |
| **Layer 1 KS** (Saturated conductivity) | mm/day | - | 10–100 | Drainage rate; high = sandy (fast), low = clay (slow) |

---

## Soil Layer Structure

### Typical APSIM Profile (7 layers)

```
Layer 1: 0–100 mm (Topsoil, root zone)
Layer 2: 100–250 mm
Layer 3: 250–450 mm
Layer 4: 450–750 mm
Layer 5: 750–1100 mm
Layer 6: 1100–1500 mm
Layer 7: 1500+ mm (Deep subsoil)
```

### Why Layers?

- **Texture changes**: Topsoil differs from subsoil (e.g., loamy top, clay bottom)
- **Root extraction**: Roots access deeper layers as crop grows; deeper layers have less water stress
- **Drainage rate**: Varies with depth; deeper layers drain more slowly
- **N availability**: Nitrate leaches to deeper layers; topsoil has more organic N

---

## Water Characteristics

### Three Critical Water Content Values

| Value | Symbol | Definition | Significance |
|-------|--------|-----------|---|
| **Lower Limit** | LL | Water content at wilting point | Crop cannot extract below this; plant available water = DUL – LL |
| **Drained Upper Limit** | DUL | Water after 2–3 days drainage | Field capacity; typical "full" water state |
| **Saturation** | SAT | All pores filled; O₂ excluded | Waterlogged; anaerobic; harmful to most crops |

### Extractable Soil Water (ESW)

```
ESW(Layer i) = (DUL – θ_current) × Thickness(i)

Total ESW = Σ ESW(Layer i) across all layers with roots
```

**Role**: Sowing decisions, water stress calculations, diagnostic for drought.

---

## Nitrogen Pools & Transformations

### Daily N Cycle

```
Organic N (humus, residue)
  ↓ [Mineralization; T- and moisture-dependent]
NH₄⁺ (ammonium pool)
  ├→ [Nitrification; aerobic]
  │   NO₃⁻ (nitrate pool)
  │    ├→ [Plant uptake]
  │    └→ [Leaching with drainage]
  └→ [Volatilization, plant uptake, immobilization]

Fertilizer N input (e.g., 160 kg/ha NO₃-N)
  ↓
Increases NO₃⁻ pool directly
```

### Key Parameters

| Parameter | Description | Typical Values |
|-----------|---|---|
| **Organic N** | Initial soil organic N | 0.1–0.3% by mass |
| **NO₃ (initial)** | Nitrate at start | 10–50 mg/kg |
| **NH₄ (initial)** | Ammonium at start | 5–15 mg/kg |
| **Mineralization rate** | Organic N → NH₄ per day | 0.01–0.05% organic pool/day |
| **Nitrification rate** | NH₄ → NO₃ per day | 10–30% NH₄ pool/day |

---

## Common Misconceptions

### 1. "Soil is static; I set it once and it doesn't change"
**Clarification**: Soil state variables (θ, NO₃, NH₄) change every day. APSIM calculates daily updates based on Weather, root uptake, leaching, and transformations.

### 2. "All 7 layers have the same water characteristics"
**Clarification**: Soil is heterogeneous. Topsoil (often sandy, high LL) differs from subsoil (often clay, low LL). Different soils require different profiles; can't use single template everywhere.

### 3. "Deeper layers don't matter; all water comes from top layer"
**Clarification**: Roots grow deep (wheat roots to 1–1.5 m); deep layers provide 30–50% of seasonal water uptake. Ignoring deep layers underestimates available water.

### 4. "N fertilizer is immediately available; NO₃ pool = applied N"
**Clarification**: NO₃ is available, but NH₄ requires nitrification (1–2 days). Organic N requires mineralization (weeks to months). Applied NO₃-N is immediately plant-available, but other N must be transformed first.

---

## Troubleshooting

### Crop water-stressed despite high rainfall

**Checklist**:
1. Are DUL/LL values correct? (Check against soil survey; typical DUL 0.35–0.50)
2. Are layers deep enough? (Wheat needs 1–1.5 m; shallow profile limits uptake)
3. Is KS (drainage) too high? (Sandy soil drains fast; water leaves before crop can use)
4. Are roots growing? (Crop must achieve full root depth to access deep water)

### Crop deficient in N despite fertilizer

**Checklist**:
1. Was fertilizer applied on correct date? (Check Manager rule date)
2. Is applied amount correct? (e.g., 160 kg/ha, not 160 g/ha)
3. Is crop demanding N? (High LAI, high growth rate)
4. Is NO₃ leaching? (High drainage + sandy soil = fast leaching)

**Diagnostic**: Export [Soil].NO₃ to spreadsheet; plot daily values. Should peak after fertilizer, then decline as crop takes up.

### Soil profile incomplete; simulation crashes

**Checklist**:
1. Are all layers defined in `.apsimx` Soil node? (Minimum 1 layer; typical 7)
2. Do all layers have DUL, LL, SAT, KS, InitialWater values?
3. Is Layer thickness > 0?

---

## Exercise References

- **Module 1** (Structure): Define a soil profile; understand layer differences
- **Module 3** (Reading Output): Track [Soil].SoilWater.ESW over time
- **Exercise A** (Fallow): 7-year water balance; see how layers interact
- **Exercise B** (N Response): Monitor [Soil].NO₃ in each layer; see N leaching

---

## Related Concepts

- **[[Soil_Layers]]** — Detailed structure of multi-layer profiles
- **[[Water_Balance]]** — Daily ESW update calculation
- **[[ESW_Extractable_Soil_Water]]** — Sowing/stress decisions based on ESW
- **[[Nitrogen_Uptake]]** — Crop N demand drives uptake from soil pools

---

## Resources

- **APSOIL database**: https://www.apsim.info/ (download Australian soil profiles)
- **ASRIS**: https://www.asris.csiro.au/ (spatial soil data Australia)
- **Soil science**: Brady, N. C., & Weil, R. R. (2017). *The Nature and Properties of Soils* (15th ed.)
