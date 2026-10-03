# Chapter 13: Surface Morphology & Microloading Effects in NAND Structures

## Overview

Surface morphology (roughness, topography) and microloading (feature-size-dependent etch rate) are critical for device performance. This chapter addresses how these phenomena arise, their impact on yield, and mitigation strategies.

---

## 13.1 Microloading: Feature Size Dependence

### 13.1.1 Microloading Mechanism

Microloading is etch rate variation with feature size in packed arrays:

```
Phenomenon:
  Large features (wide trenches): Etch faster
  Small features (narrow trenches): Etch slower
  
Physical cause:
  Radical depletion near dense features
  In packed array, F· radicals consumed faster in small gaps
  Local [F·] lower → lower etch rate

Example (density variation in die):

High-density region (packed lines):
  Line spacing: 40 nm (tight, high aspect ratio)
  Feature etch rate: R = 50 nm/min (slow)
  
Medium-density region:
  Line spacing: 60 nm
  Feature etch rate: R = 60 nm/min (moderate)

Low-density region (sparse):
  Line spacing: 100 nm (loose)
  Feature etch rate: R = 70 nm/min (fast)

Result: ±20% etch rate variation across die from microloading
```

### 13.1.2 Pressure Effects on Microloading

Pressure modulates microloading strength:

```
Low pressure (50 mTorr):
  MFP large (~5 μm) relative to feature spacing (~40 nm)
  Radicals travel far before collision
  Little depletion in dense regions
  Microloading: ±5% (minimal)
  
High pressure (150 mTorr):
  MFP small (~1 μm)
  Radicals consume locally in dense regions
  Strong depletion → slow etch in packed areas
  Microloading: ±30% (severe!)

Practical: Optimize pressure to balance ARDE and microloading
  P = 75 mTorr: ARDE = 2.5, Microloading = ±12% (compromise)
```

### 13.1.3 Compensating Microloading

Strategies to minimize microloading effects:

**Strategy 1: Increase radical generation (higher power)**
```
Higher W_coil → higher [F·] overall
Even in dense regions, enough F· available
Reduces local depletion effects
Trade-off: Higher etch rate everywhere, requires endpoint control
```

**Strategy 2: Optimize gas mixture**
```
F₂ (pure) vs. CF₄/Ar:
  F₂: Very efficient dissociation, high [F·] available
      Minimizes microloading (±3-5%)
  CF₄/Ar: Lower dissociation, microloading higher (±15-20%)
  
Use F₂ only when microloading critical (advanced nodes, tight tolerance)
```

---

## 13.2 Surface Roughness Evolution

### 13.2.1 Roughness Sources and Growth

Surface roughness increases during etch via multiple mechanisms:

```
Initial roughness (as-deposited carbon):
  Ra (roughness average): 1-2 nm (smooth)

During etch:

1. Ion-induced nanoscale texture:
   Stochastic ion impacts create random variations
   Contribution to roughness: +2-4 nm

2. Fluorocarbon polymer unevenness:
   Polymer deposits with local thickness variations
   Creates ripple/wave pattern
   Contribution: +1-3 nm

3. Grain-scale etch variation:
   Carbon sp² vs. sp³ regions etch at different rates
   Creates microscale topography
   Contribution: +1-2 nm

4. Aspect ratio effects:
   Large AR features etch slower (shadowing)
   Small AR features etch faster
   Creates undulation pattern
   Contribution: +2-5 nm

Total final roughness: Ra_final = 8-16 nm (rough, 10× initial)
```

### 13.1.2 Roughness Impact on Device

High roughness degrades device performance:

```
Effects:

1. Lithography impact:
   Rough features difficult to resolve photolithographically
   Limit to ~2 nm roughness for sub-15 nm features
   Higher roughness → lower pattern definition

2. Electrical impact:
   Rough sidewalls increase surface area
   Higher parasitic capacitance
   Slower device switching

3. Integration impact:
   Rough surfaces difficult for conformal ALD deposition
   Atomic layer deposition step coverage worse
   Process margin reduced

Production limits:
  For 15 nm features: Maximum acceptable Ra ≈ 2-3 nm
  Roughness > 5 nm causes device failure
  
This is why carbon etch recipes optimize for smoothness!
```

### 13.2.3 Roughness Minimization

Strategies to keep surface smooth:

```
Strategy 1: Optimize ion energy
  Low E_ion: Smooth etch (chemical-dominated, less sputtering roughness)
  High E_ion: Rough etch (sputtering creates nanotexture)
  
Optimal: E_ion = 50-70 eV (balance smoothness and rate)

Strategy 2: Control polymer thickness
  Thin polymer (5-10 nm): Smoother, less polymer ripple
  Achieved via: Higher ion flux at surface, lower temperature
  
Strategy 3: Use hydrogen-containing gas
  H₂ additive reduces polymer formation
  Less polymer → smoother surface
  Example: CHF₃ or CF₄ + H₂ mixture

Strategy 4: Lower temperature
  Reduced thermal motion at surface
  Smoother etch
  But slower rate (trade-off)

Production recipe for smooth etch:
  E_ion = 60 eV (moderate ion energy)
  P = 80 mTorr (moderate pressure)
  T = 10-20°C (cool, for smoother etch)
  Gas: CHF₃ or CF₄ + 2% H₂ (hydrogen improves smoothness)
  
  Result: Ra_final ≈ 3-5 nm (acceptable for 15 nm features)
```

---

## 13.3 Notching and Interface Effects

### 13.3.1 Notching at Material Interfaces

When etching stops at interface, notching can form:

```
Notching mechanism:

       Carbon
         ├─ Etch proceeds vertically
         │
Interface ├─ Etch rate suddenly changes
         │  (e.g., entering SiO₂ from carbon)
         │
         └─ Ions can scatter at interface
            Create lateral etch (sidewall undercutting)
            Notch forms (undercut ~10-50 nm)

Notching visual (cross-section):

    ╱╲        Carbon
   ╱  ╲
  │    │ Interface    ← Notch forms here
  │ ╱╲ │ 
  │╱  ╲│
   ╱╲╱╲  SiO₂

Severity:
  Mild notching: 5-10 nm undercut (acceptable)
  Severe notching: 20-50 nm undercut (device failure risk)
```

### 13.3.2 Preventing Notching

Strategies to minimize notching:

```
Strategy 1: Endpoint timing
  Stop etch as soon as endpoint reached (no over-etch)
  Prevents notching formation
  Requires good endpoint detection

Strategy 2: Two-step endpoint approach
  Step 1: Etch 95% of carbon at high rate
  Step 2: Final 5% at low rate with better endpoint control
  Minimizes over-etch, reduces notching
  
Strategy 3: Interface protection
  Add thin interfacial layer (SiO₂ or Si₃N₄) at interface
  Hard stop for selectivity
  Prevents undercutting

Production practice:
  Monitor notching on weekly test wafers
  <10 nm notching acceptable
  >20 nm notching triggers investigation
```

---

## 13.4 Summary & Key Takeaways

1. **Microloading ±10-20%**: Feature-size-dependent etch rate; varies with pressure and gas chemistry

2. **Low Pressure Reduces Microloading**: P = 50 mTorr minimizes microloading (±5%) vs. ±30% at high P

3. **Roughness Triples During Etch**: Initial 2 nm → final 8-16 nm; ion energy and polymer control critical

4. **Smoothness Targets 2-3 nm**: For 15 nm features; roughness >5 nm causes device failure

5. **Notching at Interfaces**: Material change causes lateral undercutting; <10 nm acceptable, >20 nm problematic

---

**Final Chapter:** [Chapter 14: Temperature Effects on Etch & Selectivity](./14-temperature-effects.md)

---

**Chapter 13 Development Status:** Comprehensive content complete  
**Version:** 1.0
