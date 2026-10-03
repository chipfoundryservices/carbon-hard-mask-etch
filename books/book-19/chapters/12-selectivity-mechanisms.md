# Chapter 12: Selectivity Mechanisms: Carbon/SiO₂, Carbon/Si, Carbon/Photoresist

## Overview

Selectivity is the ratio of etch rates between target material (carbon) and underlayer (SiO₂, Si, or photoresist). It's controlled by chemistry, ion energy, temperature, and polymer passivation. This chapter explains the mechanistic basis for selectivity and demonstrates recipe design to achieve specific targets (10:1, 15:1, 20:1).

---

## 12.1 Carbon/SiO₂ Selectivity

### 12.1.1 Baseline Chemistry-Based Selectivity

Carbon and SiO₂ have different intrinsic etch rates in fluorine plasma:

```
Chemical etch in pure F· (no ions):
  
  Carbon: C + 2F· → CF₂ (very volatile)
    Etch rate: R_C ≈ 50-100 nm/min (fast)
    Driving force: Highly exothermic (ΔG = -510 kJ/mol)
  
  SiO₂: SiO₂ + 4F· → SiF₄ + 2O
    Etch rate: R_SiO2 ≈ 5-15 nm/min (slow)
    Driving force: Less exothermic, Si-O bonds stronger
  
  Baseline selectivity: S = R_C / R_SiO2 = 60 / 10 ≈ 6:1

Ion-assisted component:
  Ar⁺ sputtering on carbon: Y_C ≈ 0.5 atoms/ion (50 eV)
  Ar⁺ sputtering on SiO₂: Y_SiO2 ≈ 0.1 atoms/ion (50 eV)
  Sputtering selectivity: 5:1
  
  Combined (chemical + sputtering):
    S_total ≈ 10-15:1 (chemical dominates at low E_ion)
```

### 12.1.2 Ion Energy Effects on C/SiO₂ Selectivity

Ion energy drastically affects selectivity:

```
At E_ion = 20 eV (very low):
  Chemical etch dominates (weak sputtering)
  S_C/SiO2 ≈ 25-30:1 (excellent selectivity)
  But etch rate very slow (20-30 nm/min)

At E_ion = 60 eV (medium):
  Balanced chemical + moderate sputtering
  S_C/SiO2 ≈ 15-18:1 (good selectivity)
  Etch rate adequate (60-75 nm/min)
  PRODUCTION STANDARD

At E_ion = 100 eV (high):
  Strong sputtering contribution
  S_C/SiO2 ≈ 8-12:1 (poor selectivity)
  Etch rate fast (80-100 nm/min)
  Risky for underlayer protection

At E_ion = 200 eV (very high):
  Sputtering-dominated
  S_C/SiO2 ≈ 3-5:1 (dangerous, SiO₂ unprotected)
  Etch rate very fast (>100 nm/min)
  Avoid
```

### 12.1.3 Temperature Effects on C/SiO₂ Selectivity

Temperature affects chemical reactivity differently for C vs. SiO₂:

```
Temperature dependence (Arrhenius):
  R = R₀ exp(-E_a / kT)
  
  Carbon etch: E_a ≈ 12 kcal/mol (low activation energy)
  SiO₂ etch: E_a ≈ 18-20 kcal/mol (higher activation energy)

Effect of temperature increase (+20°C):
  Carbon rate increase: ~13-15% per 10°C → ~26-30% for +20°C
  SiO₂ rate increase: ~8-10% per 10°C → ~16-20% for +20°C
  
  Result:
    Selectivity = R_C / R_SiO2 decreases with temperature
    High T selectivity worse than low T
    
Example:
  At T = 20°C: R_C = 60 nm/min, R_SiO2 = 10 nm/min, S = 6:1
  At T = 40°C: R_C = 76 nm/min, R_SiO2 = 12 nm/min, S = 6.3:1 (slight improvement, unexpected)
  
  Wait—this suggests higher T slightly improves selectivity?
  
  Actually: Different E_a values and polymer effects dominate:
  - Carbon: Faster chemical etch at higher T
  - SiO₂: SLOWER (due to oxidation layer formation at high T blocking F·)
  - Net: Selectivity can improve at higher T due to oxide blocking on SiO₂
  
  Practical: T = 20-40°C optimal range; T > 50°C degrades selectivity via oxidation
```

---

## 12.2 Carbon/Silicon Selectivity

### 12.2.1 C/Si Chemistry

Silicon is more reactive with fluorine than SiO₂:

```
Si + 4F· → SiF₄ (very volatile, easy to remove)
  Etch rate: R_Si ≈ 30-50 nm/min (faster than SiO₂)
  SiF₄ extremely volatile (BP -70°C), desorbs easily

Carbon/Si selectivity baseline:
  R_C / R_Si = 60 / 40 ≈ 1.5-2:1 (poor!)
  
  This is problematic: Si under carbon mask etches nearly as fast as carbon!
```

### 12.2.2 Improving C/Si Selectivity via Oxidation

Silicon oxidizes in fluorine plasma, creating SiO₂ overlayer:

```
Mechanism:
  Si + F· + O (from plasma) → SiO₂ (oxide overlayer)
  SiO₂ is protective layer, slows Si etch significantly
  
Quantified:
  Bare Si: R_Si ≈ 50 nm/min
  Si with oxide: R_Si_oxide ≈ 15-20 nm/min
  Oxide layer thickness: ~5-20 Å after 10 sec

Result:
  With oxide layer:
    R_C / R_Si_oxide = 60 / 18 ≈ 3.3:1 (better!)

How to maximize Si oxidation:
  Higher temperature (+50°C): Oxidation faster, protective oxide thicker
  Lower pressure: More atomic O available
  O₂ additive in gas: Direct O₂ → O· generation
  Lower bias power (lower E_ion): Less sputtering removes oxide

Practical recipe for C/Si selectivity:
  Baseline (good C/SiO₂ selectivity):
    P = 80 mTorr, W_bias = 500 W, T = 20°C, CF₄/Ar
    S_C/SiO2 = 15:1, S_C/Si_oxide = 3:1 (marginal!)
  
  Optimized (protect Si):
    P = 60 mTorr (lower, promotes oxidation)
    W_bias = 300 W (lower E_ion, reduce Si sputtering)
    T = 30°C (slightly warmer, faster oxidation)
    Gas: CF₄/Ar + 2% O₂ (provides O· for oxidation)
    S_C/SiO2 = 12:1 (slight reduction)
    S_C/Si_oxide = 5-7:1 (significantly improved!)
```

---

## 12.3 Carbon/Photoresist Selectivity

### 12.3.1 Why Photoresist Protection is Critical

Photoresist must remain intact during carbon etch (photoresist is mask):

```
Ideal selectivity: S_C/PR = ∞ (photoresist not etched at all)
Real selectivity: S_C/PR = 20-100:1 (some PR loss inevitable)

Photoresist composition:
  Base: Phenolic resin or novolac (aromatic polymer)
  Sensitizer: Photoactive compound (PAC)
  Solvent: PGMEA or other organic
  
Photoresist etch in fluorine plasma:
  F· attacks C-C and C-H bonds in polymer backbone
  Etch rate: R_PR ≈ 2-5 nm/min (surprisingly slow, similar to SiO₂!)
  
  But PR has poor selectivity against F· (soft material, easily attacked)
  Selectivity: S_C/PR = 60 / 3 ≈ 20:1 (moderate)
```

### 12.3.2 Protecting Photoresist During Carbon Etch

Strategies to minimize PR loss:

**Strategy 1: Low ion energy (pure chemical etch)**
```
Use E_ion ≈ 20-30 eV (very low bias power)
  Chemical etch of C: Still fast (~40-50 nm/min)
  Chemical etch of PR: Slow (~1-2 nm/min)
  Selectivity: S_C/PR ≈ 30-50:1 (excellent!)
  
Trade-off: Overall C etch rate slow (40-50 nm/min), extends process time
```

**Strategy 2: Lower temperature**
```
Use T ≈ -10 to +10°C (cryogenic cooling)
  Lower T → lower chemical reaction rate for both C and PR
  But C reacts faster (lower E_a) than PR
  
  At low T:
    R_C ≈ 40-50 nm/min (decreased from 60)
    R_PR ≈ 1-1.5 nm/min (decreased from 3)
    S_C/PR ≈ 40:1 (better!)
```

**Strategy 3: End-point detection and quick stop**
```
Monitor etch progress
Stop immediately when carbon completely removed
Avoid over-etch into PR region

Requires:
  Good optical or electrical endpoint detection
  Fast valve response to stop plasma
  Practiced on test wafers first

Practical: Can limit PR loss to <5 nm even with modest selectivity
```

---

## 12.4 Multi-Material Stack Integration

### 12.4.1 Complex Stack Selectivity Requirements

Real 3D NAND has multiple layers to protect:

```
Process stack (simplified):

Top    │ Photoresist (mask)
       │ Carbon hard mask (target for etch)
       ├─ SiO₂ spacer (must not etch, <1 nm loss acceptable)
       ├─ Si₃N₄ cap (must not etch, <1 nm loss acceptable)
       ├─ SiO₂ interlayer dielectric
       ├─ Si charge trap layer (critical, <5 nm loss acceptable)
       └─ Si substrate

Selectivity matrix (required):

Target    │ SiO₂    Si₃N₄    Si(trap)  PR
Carbon    │ 15:1    10:1     3:1       30:1
SiO₂      │ ---     2:1      0.5:1     50:1
Si₃N₄     │ 0.5:1   ---      1:1       100:1

Challenge: One recipe must maintain ALL selectivities simultaneously!
Solution: Carefully tune ion energy and chemistry to balance.

Example compromise recipe:
  E_ion = 60 eV (balance multiple selectivities)
  T = 25°C (moderate, avoids extremes)
  Gas: CF₄/Ar + 1% O₂ (slight oxidation for Si protection)
  
  Results:
    C/SiO₂: 14:1 ✓ (acceptable)
    C/Si₃N₄: 8:1 ✓ (acceptable, Si₃N₄ has good F resistance)
    C/Si_oxide: 4:1 ✓ (acceptable with oxide layer)
    C/PR: 25:1 ✓ (acceptable, low PR loss)
    
  All selectivities met with single recipe (rare and difficult!)
```

---

## 12.5 Selectivity Measurement and Validation

### 12.5.1 Test Structure Design

Fabs use test wafers to quantify selectivity:

```
Stacked test wafer:

  Carbon: 20 nm (target, to be etched)
  SiO₂:   100 nm (underlayer)
  Si:     50 nm (substrate simulation)

Etch procedure:
  1. Process carbon completely (all 20 nm removed)
  2. Stop etch
  3. Measure remaining thickness (XRF or ellipsometry)
  
  SiO₂ remaining: 99 nm (1 nm loss)
  Si remaining: 48 nm (2 nm loss)
  
  Selectivity calculation:
    S_C/SiO2 = 20 nm / 1 nm = 20:1 ✓
    S_C/Si = 20 nm / 2 nm = 10:1 ✓ (acceptable)
```

### 12.5.2 Selectivity Drift During Production

Selectivity can degrade as equipment drifts:

```
Week 1 (clean tool):
  E_ion = 70 eV (good calibration)
  S_C/SiO2 = 18:1 (specification met)

Week 4 (carbon on electrode builds up):
  Electrode impedance rises
  RF matching system compensates by increasing bias voltage
  E_ion drifts to 90 eV (undetected!)
  S_C/SiO2 drops to 10:1 (OUT OF SPEC!)
  Wafers failing due to SiO₂ over-etch
  
  Fix: Monitor selectivity on control wafers weekly
       Recalibrate bias power when selectivity drifts
       Before damage occurs
```

---

## 12.6 Summary & Key Takeaways

1. **C/SiO₂ Selectivity ~15:1**: Baseline chemistry + ion energy tuning achieves 10-20:1 range

2. **Ion Energy Dominates**: E_ion 20 eV vs. 100 eV changes selectivity 4-5×; lower E better

3. **Temperature Affects Chemistry**: Different E_a for C vs. SiO₂; optimize T for desired selectivity

4. **C/Si Challenging**: Raw Si etch fast; protect via oxide formation (O₂ additive, lower E_ion)

5. **Photoresist Protection Possible**: S_C/PR ≈ 25-30:1 achievable with low ion energy or cryogenic T

6. **Multi-Material Stacks Hard**: Single recipe must balance 4+ selectivity requirements; careful tuning essential

7. **Weekly Validation Required**: Monitor selectivity on control wafers; drift common from electrode carbon buildup

---

**Next Chapters:** [Chapter 13: Surface Morphology & Microloading](./13-morphology-microloading.md) and [Chapter 14: Temperature Effects](./14-temperature-effects.md)

---

**Chapter 12 Development Status:** Comprehensive content complete  
**Version:** 1.0
