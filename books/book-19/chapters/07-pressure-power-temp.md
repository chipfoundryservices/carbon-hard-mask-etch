# Chapter 7: Pressure-Power-Temperature Phase Space for Carbon Mask Etch

## Overview

The three parameters controlling carbon etch—pressure (P), RF power (W), and wafer temperature (T)—form a three-dimensional phase space. Understanding this space is essential for recipe development: which operating points deliver desired etch rate, selectivity, uniformity, and repeatability? This chapter maps the phase space, identifies stable regions, and provides practical guidance for recipe optimization.

**Learning Objectives:**
- Construct 3D P-W-T phase diagrams with etch rate and selectivity contours
- Identify stable operating windows vs. sensitive (steep gradient) regions
- Understand ion energy distribution control via pressure and bias
- Design recipes for specific selectivity targets
- Analyze robustness to equipment variations and drift

---

## 7.1 Parameter Ranges and Operating Windows

### 7.1.1 Practical Ranges for Carbon Etch

**Pressure (P):**
```
Typical range: 10-200 mTorr
Practical production window: 40-120 mTorr
Below 10 mTorr: Plasma becomes too sparse, etch rate drops dramatically
Above 200 mTorr: Pressure-driven gas convection dominates, selectivity poor
```

**RF Power:**
```
Typical range: 500-5000 W (for 300mm chamber)
Practical production window: 1000-3000 W
Below 500 W: Low plasma density, slow etch rate, difficult etch uniformity
Above 5000 W: Excessive heat, electrode damage risk, thermal runaway
Power scales with chamber size (200mm chamber: 300-2000 W typical)
```

**Temperature:**
```
Typical range: -50°C to +80°C (with cryogenic/convective cooling)
Practical production window: -10°C to +50°C
Below -50°C: Excessive cooling cost, process complexity
Above +80°C: Passive cooling limits, oxidation layer formation dominates
Production sweet spot: 0-40°C (balance of selectivity and uniformity)
```

### 7.1.2 Three Coupled Mechanisms

Three mechanisms compete in this phase space:

```
Mechanism 1: CHEMICAL ETCH
  Rate ∝ [F·] × exp(-E_a / kT)
  
  [F·] depends on:
    - Power (higher P → higher T_e → more dissociation)
    - Pressure (higher P → more collision → more radicals)
    - Temperature (higher T → higher reaction rate, exponential)
  
  Power dependence: R_chem ∝ W^0.5-0.7 (sublinear, due to plasma saturation)
  Pressure dependence: R_chem ∝ P^0.4-0.6 (sublinear, due to recombination)
  Temperature dependence: R_chem ∝ exp(-12/T) in K, E_a ≈ 12 kcal/mol

Mechanism 2: SPUTTERING (ION-ASSISTED)
  Rate ∝ Y_sputter × J_ion
  
  Y_sputter depends on: Ion energy E_ion (increases 20-100 eV, saturates >100 eV)
  J_ion depends on: Power (higher W → higher ion flux)
  
  Power dependence: R_sputter ∝ W (linear in ion flux)
  Pressure dependence: Inverse relationship (higher P → lower E_ion)
  Temperature dependence: Weak (mainly kinetic, <5%/°C)

Mechanism 3: FLUOROCARBON POLYMER
  Deposition ∝ T_e × [CF] (radical polymerization)
  Removes selectivity, but essential for anisotropy
  
  Power dependence: More polymer at higher power (more CF₂ generation)
  Pressure dependence: Moderate effect
  Temperature dependence: Higher T → less polymer (thermal decomposition)
```

---

## 7.2 Phase Diagrams and Contour Maps

### 7.2.1 Etch Rate Phase Space

**2D slice: Etch rate vs. Pressure and Power (fixed T = 20°C)**

```
Power (W)
 3000 |                   ╭─ 80 nm/min ─╮
      |                 ╱ ╱ 70 nm/min  ╲ ╲
 2000 |              ╱ ╱ 60 nm/min      ╲ ╲
      |           ╱ ╱ 50 nm/min         ╲ ╲
 1000 |        ╱ ╱ 40 nm/min            ╰─╯ Stable
      |      ╱ ╱ 30 nm/min                    region
      |   ╱ ╱ 20 nm/min
    0 |_╱_╱_________________________
      0    50   100   150   200
              Pressure (mTorr)

Key features:
  - Peak etch rate: ~80 nm/min at (P=75 mTorr, W=2500W, T=20°C)
  - Etch rate increases linearly with power in this window
  - Etch rate rises then plateaus with pressure (MFP effects)
  - Ridge-line (contour of peak etch rate): P~75 mTorr independent of W
```

**3D surface: Etch rate as function of P, W, T**

The complete 3D surface shows:
- Linear increase with power (ion-assisted contribution)
- Sublinear increase with pressure (radical generation + recombination balance)
- Exponential increase with temperature (Arrhenius temperature dependence)

### 7.2.2 Selectivity Phase Space

**2D slice: C/SiO₂ Selectivity vs. Pressure and Ion Energy**

```
Selectivity (C/SiO₂)
   50 |           ╱╲___
      |        ╱╱    ╲___
   30 |     ╱╱          ╲___
      |  ╱╱                ╲___
   10 |╱╱
      |
    0 |_________________________
      0   30   60   90  120  150
                E_ion (eV)

For fixed pressure, higher ion energy (higher RF bias):
  - Selectivity peaks at ~50-70 eV
  - Lower E_ion: Selectivity excellent (30-50:1) but etch rate poor
  - Higher E_ion: Selectivity moderate (10-15:1) but etch rate good
  - Sweet spot: 60-80 eV for balanced selectivity/rate
```

**Effect of pressure on selectivity:**

```
At same ion energy (E_ion = 70 eV):

Low pressure (40 mTorr):
  Ion energy higher (less collision, ions keep energy)
  Selectivity: C/SiO₂ ≈ 20-25:1 (good)

Medium pressure (80 mTorr):
  Ion energy moderate
  Selectivity: C/SiO₂ ≈ 15-20:1 (balanced)

High pressure (160 mTorr):
  Ion energy lower (more collision, ions slow down)
  Selectivity: C/SiO₂ ≈ 8-12:1 (poor, ion-assisted sputtering weak)
```

### 7.2.3 Temperature Effects on Selectivity

Temperature affects selectivity through multiple mechanisms:

```
At LOW temperature (-10°C):
  - Chemical etch slower (lower reaction rate)
  - Polymer deposition higher (less thermal decomposition)
  - Oxidation layer formation suppressed
  - Selectivity: Higher, ~20-25:1
  
  Trade-off: Slower etch rate means longer recipe time

At MEDIUM temperature (+20°C):
  - Balanced chemical + polymer effects
  - Production standard
  - Selectivity: Moderate, ~15-18:1
  - Etch rate: Good, ~50-70 nm/min

At HIGH temperature (+60°C):
  - Chemical etch fast (high reaction rate)
  - Polymer decomposition significant
  - Oxidation layer thick (carbon oxidized during etch)
  - Selectivity: Lower, ~10-12:1
  - Etch rate: Very fast, ~100+ nm/min (but uncontrolled)
```

---

## 7.3 Ion Energy Distribution Control

### 7.3.1 Self-Bias Voltage and Ion Energy

The RF bias creates potential difference that accelerates ions:

```
Self-bias voltage:
  V_bias = √(I_bias / (f × C × Φ_ion))
  
where:
  I_bias = bias RF current (proportional to W_bias)
  f = bias frequency (2 MHz typical)
  C = electrode capacitance
  Φ_ion = ion flux

Practical range:
  Bias power 200-1000 W → V_bias ≈ -100 to -500V
  Ion energy: E_ion ≈ e × V_bias ≈ 100-500 eV
  (Large variation, requires careful tuning)
```

**Ion energy vs. bias power:**

```
Ion Energy (eV)
   500 |                    ╭─ Runaway
       |                  ╱╱
   300 |               ╱╱    (unstable region)
       |            ╱╱
   100 |         ╱╱
       |      ╱╱ (stable)
     0 |___╱╱_________________________
       0    200   400   600   800  1000
                W_bias (W)

Key observations:
  - Roughly linear relationship: E_ion ≈ 0.3 × W_bias (in V and eV)
  - Below 100 W: Sparse ions, difficult control
  - 200-600 W: Production window, smooth control
  - Above 800 W: Thermal runaway risk
  - Curve slope changes with pressure (higher P flattens curve)
```

### 7.3.2 Ion Energy Distribution Width

Real plasma has distribution of ion energies, not single value:

**Maxwell-Boltzmann distribution:**

```
Most ions concentrated around E_peak ≈ e × V_bias
But tails extend to:
  - Low energy: E_min ≈ 0 eV (slow ions from recombination)
  - High energy: E_max ≈ 2-3 × E_peak (fast ions from collisions)
  
FWHM (full width at half maximum) of distribution:
  ΔE ≈ 0.3 × E_peak
  
Example: E_peak = 70 eV → FWHM ≈ 21 eV
  Range: 30-110 eV at FWHM boundaries
  Effective range: 0-200+ eV including tails
```

**Production implication:**

Broad IED means some ions are too slow (<20 eV, no sputtering), others too fast (>150 eV, poor selectivity).

---

## 7.4 Process Windows and Stability Analysis

### 7.4.1 Flat vs. Steep Regions

Recipe robustness depends on region of phase space:

**Flat regions (good):**
```
Gradual etch rate change with P, W, T
Example: "Plateau" region where etch rate ≈ constant over ±20% parameter variation

Advantage:
  - Small equipment variations → small etch rate changes
  - ±10% power variation → ±2-3% etch rate change
  - ±5% pressure variation → ±1-2% etch rate change
  - Easy to tune and maintain
  
Drawback:
  - Lower etch rate (efficiency trade-off)
  - Slower throughput
```

**Steep regions (sensitive):**
```
Rapid etch rate change with P, W, T
Example: "Peak etch rate" region where rate rises linearly with power

Advantage:
  - Higher etch rate (good throughput)
  - Better plasma efficiency
  
Drawback:
  - ±10% power variation → ±10-20% etch rate change
  - Recipe must be precisely tuned
  - Drifts in equipment parameters cause yield variation
  - Difficult to maintain and debug
```

### 7.4.2 Robustness Analysis Example

**Scenario: Target etch rate = 50 nm/min, selectivity C/SiO₂ = 18:1**

Option A: Operate in flat region
```
Recipe:
  Pressure: 100 mTorr
  Power: 1500 W
  Temperature: 20°C
  Predicted etch rate: 50 nm/min
  Predicted selectivity: 18:1

Equipment variations (realistic 1-sigma):
  Pressure ±5%: 95-105 mTorr
  Power ±8%: 1380-1620 W
  Temperature ±3°C: 17-23°C
  
Resulting etch rate: 48-52 nm/min (±4%, acceptable)
Selectivity variation: 17-19:1 (within ±5% target)
Yield impact: Low (most wafers within spec)
```

Option B: Operate in steep region
```
Recipe:
  Pressure: 55 mTorr (near peak efficiency)
  Power: 2200 W (high power)
  Temperature: 15°C (cool for selectivity)
  Predicted etch rate: 50 nm/min (at operating point)
  Predicted selectivity: 19:1

Equipment variations (same ±5%, ±8%, ±3°):
  Resulting etch rate: 42-58 nm/min (±16%, problematic!)
  Selectivity variation: 17-21:1 (outside ±5% tolerance)
  Yield impact: High (many wafers out of spec)
```

**Conclusion:** Flat regions sacrifice peak performance but gain robustness. High-volume fabs prefer robustness; cutting-edge labs prefer peak performance.

---

## 7.5 Multi-Parameter Recipes

### 7.5.1 Selectivity Tuning Via Temperature

Temperature is a powerful tuning knob for selectivity without changing etch rate much:

```
Recipe foundation (baseline at 20°C):
  P = 80 mTorr
  W = 2000 W
  T = 20°C
  R_etch = 65 nm/min
  S_C/SiO₂ = 16:1

Selectivity boost (reduce temperature):
  T = -10°C (cryogenic cooling)
  R_etch = 60 nm/min (slight decrease, ~8%)
  S_C/SiO₂ = 22:1 (38% selectivity improvement!)
  
  Use for: Critical selectivity requirement, where underlayer protection essential

Speed boost (increase temperature):
  T = +40°C (moderate heating)
  R_etch = 85 nm/min (30% increase)
  S_C/SiO₂ = 12:1 (25% selectivity reduction)
  
  Use for: High throughput requirement, non-critical selectivity
```

### 7.5.2 ARDE Compensation Via Pressure

Aspect ratio-dependent etch rate varies with pressure:

```
Recipe for uniform etch across 1:1 to 20:1 aspect ratios:
  
  For large aspect ratios (20:1), low pressure needed for penetration:
    P = 50 mTorr, W = 2000 W
    Top etch: 80 nm/min
    Bottom etch: 60 nm/min (25% ARDE)
    
  For small aspect ratios (1:1), pressure can be higher:
    P = 100 mTorr, W = 2000 W
    Etch uniform: ~65 nm/min (no ARDE)
    
  Compromise recipe (mixed aspect ratios):
    P = 70 mTorr, W = 2000 W
    Top/large AR: 75 nm/min
    Bottom/small AR: 65 nm/min
    ARDE: ~15% (acceptable compromise)
```

---

## 7.6 Recipe Development Workflow

### 7.6.1 Design of Experiments (DOE)

Systematic approach to recipe optimization:

```
Step 1: Choose target specifications
  - Etch rate: 60 ±5 nm/min
  - Selectivity: 18 ±2:1 (C/SiO₂)
  - Uniformity: ±5%
  - Linewidth: 15 ±1 nm (critical feature)

Step 2: Select parameter ranges
  - Pressure: 50-100 mTorr (5 levels)
  - Power: 1500-2500 W (5 levels)
  - Temperature: 0-40°C (3 levels)
  - Total combinations: 5 × 5 × 3 = 75 experiments

Step 3: DOE matrix (factorial or fractional design)
  - Full factorial (75 exp): Comprehensive but expensive
  - Fractional factorial (16 exp): ~1/5 effort, captures main effects
  - Response surface (20 exp): Optimized sampling, finds optima

Step 4: Run experiments
  - Each condition: 3 test wafers (average results)
  - Measure etch rate, selectivity, uniformity
  - Record all data systematically

Step 5: Analyze results
  - Main effects plot: Which parameter most affects etch rate?
  - Interaction plot: Do parameters interact?
  - Contour map: Find optimal region
  - Model fitting: Predict etch rate from P, W, T (polynomial fit)

Step 6: Validation
  - Run 5 wafers at predicted optimal recipe
  - Verify etch rate, selectivity, uniformity
  - Adjust if needed (fine-tuning)
```

### 7.6.2 Sensitivity Analysis

Quantifies which parameters most affect specifications:

```
Sensitivity matrix (normalized, ∂PerformanceParameter / ∂RecipeParameter):

                    ∂(Etch rate)  ∂(Selectivity)  ∂(Uniformity)
∂Pressure (±5%)         0.5           -0.6            0.2
∂Power (±8%)            0.8            0.1            0.3
∂Temperature (±3°C)     0.2           -0.9            0.1

Interpretation:
  - Etch rate most sensitive to power (coeff = 0.8)
  - Selectivity most sensitive to temperature (coeff = -0.9, negative means inverse)
  - Uniformity most sensitive to power
  
  Implication: Control power carefully for etch uniformity; control temperature
  carefully for selectivity target.

Robustness scoring:
  Sum of absolute sensitivities = 0.8 + 0.1 + 0.3 = 1.2 (moderate)
  Lower total = more robust recipe
  Goal: <1.0 for production recipes
```

---

## 7.7 Summary & Key Takeaways

1. **3D Phase Space is Complex**: P, W, T couple in non-linear ways; simple linear models insufficient

2. **Etch Rate Peaks at ~75 mTorr**: Optimal pressure balances radical generation (higher P) vs. recombination (too high P)

3. **Selectivity Improved at Low T**: Temperature -10°C improves selectivity 30-40% vs. +40°C, at cost of ~10% etch rate reduction

4. **Ion Energy Windows Exist**: Selectivity optimal at 60-80 eV; lower improves selectivity, higher increases rate

5. **Flat Regions > Peak Performance**: For production volume, flat phase-space regions (robust to ±10% variation) better than peak efficiency

6. **ARDE Compensation Needs Pressure**: Low pressure (50 mTorr) improves aspect ratio penetration; pressure is primary ARDE tuning knob

7. **Temperature is Powerful Tool**: ±1°C temperature change ≈ ±10% etch rate change; use for fine selectivity tuning without affecting power budget

---

## Study Questions

1. Plot etch rate vs. power (300-2500 W) at constant P=80 mTorr, T=20°C. Explain the shape (linear vs. sublinear).

2. Design recipes for two extreme cases: (a) maximum etch rate, (b) maximum selectivity. Trade-offs?

3. Calculate ion energy distribution width (FWHM) for 70 eV peak energy. What fraction of ions are below 20 eV (unable to sputter)?

4. Design DOE matrix: 3 pressure levels, 3 power levels, 2 temperature levels. What are minimum experiments for fractional factorial?

5. Analyze robustness: If power drifts ±8%, what phase-space region minimizes etch rate variation?

---

**Next Chapter:** [Chapter 8: Chamber Wall Coatings & Carbon Film Management](./08-chamber-coatings.md)

---

**Chapter 7 Development Status:** Comprehensive content complete  
**Last Updated:** October 3, 2026  
**Version:** 1.0 (Complete technical chapter)
