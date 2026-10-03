# Chapter 11: Ion Energy & Ion Flux Distribution Control

## Overview

Ion energy (measured in eV, controlled by RF bias voltage) and ion flux (number of ions reaching wafer per second) are independent parameters governing etch selectivity, anisotropy, and rate. This chapter addresses practical control of both: measuring ion energy distributions, designing recipes for specific selectivity targets, and implementing closed-loop feedback.

**Learning Objectives:**
- Measure ion energy distribution (IED) at wafer surface
- Design recipes for specific selectivity targets (10:1, 15:1, 20:1 C/SiO₂)
- Understand trade-offs between selectivity and etch rate
- Control ion flux independently from ion energy
- Implement ion energy feedback for production uniformity
- Predict and compensate for energy distribution changes

---

## 11.1 Ion Energy Measurement and Characterization

### 11.1.1 Retarding Potential Analyzer (RPA)

Standard diagnostic for measuring ion energy distribution at wafer surface:

```
Principle:
  Ions extracted through small orifice (~1 mm diameter)
  Pass through retarding potential (electrostatic barrier)
  Only ions with E ≥ e×V_retard reach collector
  By varying V_retard, construct E vs. ion current spectrum

Setup:
  Plasma → [Extraction orifice] → [Retarding grid at potential V_retard]
                                       ↓ (only E ≥ e×V_retard pass)
                                 [Collector] → Faraday cup measures current

Measurement procedure:
  Scan V_retard from 0V to 500V
  Measure collector current I(V) at each voltage
  Plot: Current vs. Retarding potential
  
  dI/dV ∝ distribution of ions (RPA spectrum)
```

**Example RPA Result:**

```
Ion Current (nA)
     200 |
         |    ╱╲
     150 |   ╱  ╲      Broad distribution
         |  ╱    ╲╲    Peak near 80 eV
     100 | ╱      ╲╲   FWHM ≈ 40 eV
         |╱        ╲╲
      50 |          ╲╲___
         |              ╲____
       0 |____________________
         0    50   100   150   200  Voltage (eV)
         
Peak energy: ~80 eV
Mean energy: ~75 eV
FWHM: ~35 eV (±17.5 eV)
Tail extends to 200+ eV
```

### 11.1.2 Energy Distribution Interpretation

Broadness of IED affects etch results:

```
Narrow distribution (FWHM ~20 eV):
  Most ions at same energy
  Better selectivity (well-defined sputtering)
  But harder to control during drift
  Achieved with low pressure, good RF stability

Broad distribution (FWHM ~50 eV):
  Large fraction of slow ions (<20 eV, don't sputter)
  Large fraction of fast ions (>150 eV, poor selectivity)
  Robustness (some ions always available)
  But poorer selectivity control

Practical production:
  Target FWHM ~30-40 eV (balance of control and robustness)
  Peak energy tuned via bias power (E_peak ≈ 0.3 × W_bias, in V and eV)
  
Ion energy thresholds:
  <20 eV: Chemical etch only (no sputtering)
  20-50 eV: Threshold sputtering begins
  50-100 eV: Optimal range (good yield, acceptable selectivity)
  100-200 eV: High sputtering yield (poor selectivity)
  >200 eV: Ion damage risk, unnecessary energy
```

---

## 11.2 Ion Flux Control and Measurement

### 11.2.1 Ion Current Measurement

Total ion flux measured via Faraday cup or AC probes:

```
Ion flux definition:
  Φ_ion = J_ion / e
  
where:
  J_ion = ion current (Amperes)
  e = elementary charge (1.6 × 10⁻¹⁹ C)
  Φ_ion = ions per second

Measurement setup:
  Faraday cup (inside chamber, collecting ions directly)
  or
  AC probe (downstream, measuring ion saturation current)

Typical values for carbon etch:
  Ion current: 0.5-2 A (depends on power, pressure, chamber size)
  For 300mm wafer (~0.07 m² area):
  
  Ion current density: J = 0.5-2 A / 0.07 m² = 7-30 A/m²
  
  Ion flux: Φ_ion = J / e = (10 A/m²) / (1.6×10⁻¹⁹ C)
                   ≈ 6 × 10¹⁹ ions/(m²·s)
```

### 11.1.2 Controlling Ion Flux

Ion flux controlled primarily by RF coil power:

```
Relationship (approximate):
  Φ_ion ∝ W_coil^0.8 (sublinear, due to plasma saturation)

Example:
  W_coil = 1000 W: Φ_ion ≈ 3 × 10¹⁹ ions/(m²·s) (low, slow etch)
  W_coil = 1500 W: Φ_ion ≈ 4.5 × 10¹⁹ ions/(m²·s)
  W_coil = 2000 W: Φ_ion ≈ 5.5 × 10¹⁹ ions/(m²·s)
  W_coil = 3000 W: Φ_ion ≈ 7 × 10¹⁹ ions/(m²·s) (high, fast etch)

Ion flux vs. etch rate:
  Higher Φ_ion (more ions) → higher etch rate directly
  But also affects temperature (more ion bombardment = more heat)
  Combined effect: R_etch ∝ Φ_ion × sputtering_yield
```

**Independent control:**
```
Example: Want high ion flux but low ion energy (for selectivity)

Option A: Low bias power, high coil power
  W_coil = 2500 W → high Φ_ion (good for rate)
  W_bias = 200 W → low E_ion (~20 eV, excellent selectivity)
  Result: Fast etch (70 nm/min) with high selectivity (~20:1)
  Trade-off: Selectivity limited to range starting at 20 eV (no lower)

Option B: High bias power, moderate coil power  
  W_coil = 1500 W → moderate Φ_ion (slower etch)
  W_bias = 800 W → high E_ion (~150 eV, poor selectivity)
  Result: Slow etch (40 nm/min), poor selectivity (~10:1)
  Trade-off: Selectivity sacrificed for controllability

Optimal (balance):
  W_coil = 2000 W, W_bias = 500 W
  Φ_ion ≈ 5.5 × 10¹⁹ ions/(m²·s)
  E_ion ≈ 80 eV
  R_etch ≈ 65 nm/min
  Selectivity ≈ 15:1 (good balance)
```

---

## 11.3 Selectivity Control via Ion Energy

### 11.3.1 Selectivity Tuning Strategy

Design recipes to achieve specific C/SiO₂ selectivity targets:

```
Target: C/SiO₂ selectivity = 18:1 (high selectivity requirement)

Step 1: Identify ion energy for 18:1 selectivity
  From prior characterization or literature:
  E_ion = 60 eV → S = 25:1 (too high, slow etch)
  E_ion = 70 eV → S = 18:1 (target achieved!)
  E_ion = 80 eV → S = 15:1 (too low, risky)
  
  Decision: Target E_ion = 70 eV

Step 2: Set bias power to achieve E_ion = 70 eV
  Relationship: E_ion (eV) ≈ 0.3 × W_bias (in W)
  Required: W_bias = 70 / 0.3 ≈ 230 W
  But actual curve may have offset, so measure and adjust

Step 3: Measure on test chamber
  Set W_bias = 230 W
  Use RPA to measure actual E_ion
  If E_ion measured = 65 eV (too low):
    Increase W_bias to 270 W, re-measure
  If E_ion measured = 75 eV (too high):
    Decrease W_bias to 190 W, re-measure
  
  Iterate until E_ion ≈ 70 eV measured

Step 4: Verify selectivity on test wafers
  Run etch on test structure (C on SiO₂)
  Stop etch at predetermined time
  Measure C and SiO₂ etch depth:
    C etch depth: 200 nm
    SiO₂ etch depth: 12 nm
    Measured selectivity: 200/12 ≈ 16.7:1 ≈ 17:1 (good!)
    
  If selectivity off:
    Check ion energy (RPA)
    Verify chemistry (gas mix correct?)
    Check temperature (may affect selectivity)

Step 5: Production deployment
  Use W_bias = 240 W (empirical best value)
  Monitor weekly on control wafers
  If selectivity drifts: Recalibrate bias power
```

### 11.3.2 Selectivity vs. Etch Rate Trade-off

Recipe optimization requires balancing competing goals:

```
Parameter sweep (matrix of recipes):

Ion Energy    Etch Rate    Selectivity   Preferred Use
(eV)          (nm/min)     (C/SiO₂)      
30-40         30-40        25-30:1       Selectivity-critical, slow throughput
50-60         45-55        18-20:1       Precision, good selectivity
70-80         60-75        12-15:1       Balanced, production standard
90-100        75-90        10-12:1       Speed-optimized, marginal selectivity
150+          100-120      5-8:1         Dangerous, poor selectivity

Production strategy matrix:

Need high selectivity (20+:1)?
  → Use E_ion = 30-40 eV
  → Sacrifice etch rate (30-40 nm/min)
  → Extend process time to compensate

Need fast throughput (>80 nm/min)?
  → Use E_ion = 90-100 eV
  → Accept lower selectivity (10-12:1)
  → Risk underlayer damage, require careful endpoint tuning

Balanced production (most common):
  → Use E_ion = 70-80 eV
  → Achieve 60-75 nm/min etch rate
  → Maintain 12-15:1 selectivity (adequate for most applications)
  → Good cost-benefit for high-volume fab
```

---

## 11.4 Ion Flux Uniformity Across Wafer

### 11.4.1 Spatial Ion Flux Variation

Ion flux varies across 300mm wafer:

```
Plasma density (basis of ion flux) non-uniform in CCP:
  
  300mm wafer (top view):
  
        ╔═════════════════════════╗
        ║  Ion flux density map    ║
        ║  (hot/cold colors)       ║
        ║  ╔═══════════════════╗   ║
        ║  ║ Red (high)        ║   ║
        ║  ║  ╱╱╱╱╱╱╱╱╱╱╱╱╱╱  ║   ║
        ║  ║ /  ╭─────────╮  / ║   ║
        ║  ║/   │ Orange  │ / ║   ║
        ║  ║    │ (mid)   │  ║   ║
        ║  ║\   │         │ / ║   ║
        ║  ║ \  ╰─────────╯ / ║   ║
        ║  ║  ╲╱╱╱╱╱╱╱╱╱╱╱╱╱  ║   ║
        ║  ║ Yellow/Blue     ║   ║
        ║  ║ (low at edges)  ║   ║
        ║  ╚═══════════════════╝   ║
        ╚═════════════════════════╝

Typical ion flux profile (cross-section):
  
  Ion current density (mA/cm²)
        15 |      ╱╲___
           |    ╱╱     ╲
        10 |  ╱╱       ╲╲
           | ╱╱         ╲╲
         5 |╱╱           ╲╲___
           |                  ╲___
         0 |_____________________
           0    50   100  150   Center
           Edge          Radius (mm)

Center flux: ~15 mA/cm²
Mid-radius: ~10 mA/cm²
Edge: ~5 mA/cm²
Non-uniformity: Center/Edge = 3:1 (severe!)
```

### 11.4.2 Compensating for Ion Flux Non-Uniformity

```
Strategy 1: Design showerhead to compensate
  Higher gas flow to edges (to increase ion generation at edges)
  Lower gas flow to center (to reduce center over-etch)
  Can achieve ±10-15% uniformity

Strategy 2: Use neutral magnetic field
  Magnetic field shapes plasma confinement
  Can redistribute ion flux spatially
  Effective but complex (cost, reliability concerns)

Strategy 3: Accept non-uniformity and compensate electrically
  Measure ion flux spatially (multi-point probes)
  Adjust RF power distribution (if available)
  Most practical for existing chambers

Strategy 4: Process compensation
  Design multi-step etch with varying settings
  Early step: Low power (less affected by uniformity)
  Late step: Controlled to equalize endpoints
  Common in high-precision recipes
```

---

## 11.5 Ion Energy Feedback and In-Situ Control

### 11.5.1 Real-Time Ion Energy Monitoring

Advanced chambers monitor ion energy during etch:

```
Sensor: Fast ion energy analyzer (simplified RPA or retarding field)
  Mounted in chamber wall
  Scans ion energy distribution every 5-10 seconds
  Reports peak energy and distribution width

Feedback loop:
  1. Measure E_ion (peak from analyzer)
  2. Compare to setpoint (e.g., 70 eV for selectivity = 18:1)
  3. Error: ΔE = E_measured - E_setpoint
  4. Adjust W_bias: ΔW_bias = K × ΔE
  5. Repeat every update interval
  
Example:
  Setpoint E = 70 eV
  Measured E = 75 eV (too high, selectivity declining)
  Error: ΔE = +5 eV
  Correction: Reduce W_bias by 5 / 0.3 ≈ 17 W
  New W_bias = 225 - 17 = 208 W
  Re-measure after 1 second:
    E = 71 eV (close to target)
    Continue iterating until E ≈ 70 eV
```

### 11.5.2 Handling Energy Drift During Recipe

Ion energy can drift during etch due to:
- Electrode carbon deposition (increases impedance)
- Temperature changes (wafer heats up)
- Pressure drift (throttle valve aging)

```
Without feedback (open-loop):
  Time 0-30s: E_ion ≈ 70 eV (setpoint)
  Time 30-60s: E_ion rises to 85 eV (impedance change)
  Time 60-120s: E_ion → 95 eV (continued drift)
  
  Result:
    Early etch: Good selectivity (18:1)
    Late etch: Poor selectivity (12:1, risky)
    Overall selectivity: Inconsistent, yield variable

With ion energy feedback:
  Time 0-30s: E_ion ≈ 70 eV (setpoint)
  Time 30s: E_ion rises to 85 eV (detected)
            W_bias reduced by 50 W → E_ion corrected to 71 eV
  Time 60s: E_ion drifts to 82 eV (detected)
            W_bias adjusted → E_ion corrected to 70 eV
  Time 120s: E_ion stable ≈ 70 eV throughout
  
  Result:
    Selectivity constant: 18±1:1 throughout etch
    Yield consistent, excellent process control
```

---

## 11.6 Summary & Key Takeaways

1. **Ion Energy Measured via RPA**: Retarding potential analyzer shows distribution; mean energy ~70-80 eV typical

2. **E_ion ≈ 0.3 × W_bias**: Quantitative relationship enables bias power targeting for specific ion energies

3. **Selectivity Peaks at 60-70 eV**: Lower E better selectivity; higher E faster etch but risky

4. **Ion Flux Controlled via Coil Power**: Higher W_coil → higher Φ_ion → faster etch rate

5. **Independent Control Possible**: Can achieve high flux with low energy (fast + selective), or vice versa

6. **Ion Flux Non-Uniform ±3:1**: Edge/center variation requires showerhead compensation or process adjustment

7. **Feedback Essential for Precision**: Real-time E_ion monitoring prevents drift, maintains selectivity throughout etch

---

**Next Chapter:** [Chapter 12: Selectivity Mechanisms (multi-material)](./12-selectivity-mechanisms.md)

---

**Chapter 11 Development Status:** Comprehensive content complete  
**Version:** 1.0
