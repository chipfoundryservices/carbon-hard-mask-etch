# Chapter 10: Aspect Ratio Dependent Etching (ARDE) in Vertical NAND Stacks

## Overview

Aspect Ratio Dependent Etch (ARDE) is the most challenging phenomenon in 3D NAND carbon hard mask patterning. In 100:1+ aspect ratio trenches, etch rate varies 2-10× from top (opening) to bottom (depth). This creates linewidth variation, pattern distortion, and yield loss. Unlike silicon etch where ARDE is manageable, carbon etch ARDE is severe and requires sophisticated compensation strategies.

**Learning Objectives:**
- Understand fundamental ARDE physics (ion depletion, neutral shadowing, redeposition)
- Quantify etch rate variation with aspect ratio
- Design ARDE compensation strategies (pressure, bias power, pulsed plasma)
- Implement real-time feedback correction
- Predict etch time distributions for complex structures
- Optimize recipes for uniform 3D pattern definition

---

## 10.1 ARDE Physics Fundamentals

### 10.1.1 Three Mechanisms Contributing to ARDE

ARDE in deep trenches results from three coupled mechanisms:

**Mechanism 1: Ion Depletion (Most Significant)**

```
High-aspect-ratio trench:
  Top (opening):     Abundant ion flux (Φ_ion ≈ full plasma density)
  Mid-depth:         Reduced ion flux (Φ_ion ≈ 80% due to collisions)
  Deep trench:       Severely depleted (Φ_ion ≈ 10-20% of plasma density)

Physics:
  Ions accelerated from sheath enter trench
  First ions strike top/sidewalls, scatter
  Remaining ions continue downward, but MFP limited (~1-10 μm at process pressure)
  Collision frequency high in deep trench
  Ion mean free path: λ_ion ≈ 1-5 μm
  Trench width: w ≈ 50 nm
  Ratio: λ_ion / w ≈ 100:1 to 1000:1 (ballistic in horizontal, diffusive in vertical)

Ion flux variation:
  Φ_ion(depth=0) = J_ion / e ≈ 10¹⁷ ions/(m²·s)
  Φ_ion(depth=1μm) ≈ 0.9 × Φ_ion(0)
  Φ_ion(depth=10μm) ≈ 0.5 × Φ_ion(0)
  Φ_ion(depth=50μm) ≈ 0.1 × Φ_ion(0)

Consequence: Ion-assisted etch component drops dramatically, etch rate falls
```

**Mechanism 2: Neutral Radical Shadowing**

```
Neutral fluorine radicals (F·) travel straight-line (ballistic) in trench

Shadowing geometry:
         ╭─ Showerhead
         │  F· radicals rain down
         ▼
    ┌────────┐
    │ Trench │ Width: 50 nm
    │   ▼▼▼  │ F· radicals enter from top
    │   ▼▼▼  │ But trench sidewalls block some radicals
    │   ▼▼▼  │ Deep in trench, only direct line-of-sight F· reach bottom
    │   ▼▼   │
    │   ▼    │ At depth d, "shadow" region grows
    │   │    │ Effective F· flux reduced by geometric factor
    └────────┘
         Bottom (2000+ nm deep)

Geometric reduction:
  Effective F· flux = Φ_F(surface) × [1 - (d/d_max)²]  (approximate)
  
  At surface (d=0): F· flux = 100%
  At d = d_max/2: F· flux ≈ 75%
  At d = d_max: F· flux ≈ 0% (complete shadowing)
  
For 2000 nm depth:
  Top (0-500 nm): F· flux ≈ 95%
  Mid (500-1500 nm): F· flux ≈ 60%
  Bottom (1500-2000 nm): F· flux ≈ 20%
```

**Mechanism 3: Fluorocarbon Polymer Redeposition**

```
During etch, fluorocarbon polymer deposits on sidewalls.
Polymer layer thickness varies with depth:

Top of trench:
  - High ion flux → efficient polymer removal by sputtering
  - Polymer layer: ~5-10 nm (thin, doesn't block much)
  
Mid-depth:
  - Moderate ion flux → partial polymer removal
  - Polymer layer: ~15-25 nm (thicker, provides passivation)
  
Deep trench (bottom):
  - Low ion flux → poor polymer sputtering
  - Polymer accumulation: ~40-80 nm (very thick, blocks F· radicals)

Effect of thick polymer:
  Thick polymer layer (>30 nm) acts as diffusion barrier
  F· must diffuse through polymer to reach carbon
  Diffusion-limited etch rate: R ∝ D / thickness
  
  Bottom etch rate ≈ 10-20% of top rate (due to thick polymer)
```

---

## 10.2 Quantifying ARDE: Etch Rate Ratio

### 10.2.1 ARDE Factor Definition

ARDE factor measures etch rate variation with aspect ratio:

```
ARDE = R_top / R_bottom

where:
  R_top = etch rate at trench top (opening)
  R_bottom = etch rate at trench floor (depth)

Examples:

Small aspect ratio (1:1, 50 nm feature in 50 nm depth):
  R_top ≈ 80 nm/min
  R_bottom ≈ 75 nm/min
  ARDE ≈ 1.07 (negligible)

Moderate aspect ratio (10:1, 50 nm wide, 500 nm deep):
  R_top ≈ 80 nm/min
  R_bottom ≈ 50 nm/min
  ARDE ≈ 1.60 (40% variation)

High aspect ratio (50:1, 50 nm wide, 2500 nm deep):
  R_top ≈ 80 nm/min
  R_bottom ≈ 15 nm/min
  ARDE ≈ 5.3 (80% variation!)

Extreme aspect ratio (100:1, 50 nm wide, 5000 nm deep):
  R_top ≈ 80 nm/min
  R_bottom ≈ 5 nm/min
  ARDE ≈ 16 (catastrophic 94% variation)
```

### 10.2.2 ARDE Prediction Models

Empirical models predict ARDE based on pressure and other parameters:

```
Simple model (pressure-dependent):
  ARDE(P) = A × P^(-0.5) + B
  
  At low pressure (50 mTorr): ARDE ≈ 2.5
  At medium pressure (100 mTorr): ARDE ≈ 1.8
  At high pressure (150 mTorr): ARDE ≈ 1.5
  
  Explanation: Lower pressure → longer MFP → radicals penetrate deeper → smaller ARDE

More complex model (multi-parameter):
  ARDE = f(AR, P, W_bias, T_surface, [F·], coating_thickness)
  
  Factors increasing ARDE:
    - Aspect ratio (AR) — main driver
    - Higher pressure P (reduces ion penetration)
    - Lower bias power W_bias (less ion-assisted etch at depth)
    - Higher polymer coating (blocks radicals)
  
  Factors decreasing ARDE:
    - Lower pressure (longer MFP)
    - Higher bias power (ions penetrate better)
    - Lower temperature (less polymer, more reactivity uniformity)
```

---

## 10.3 ARDE Compensation Strategies

### 10.3.1 Pressure Modulation

Pressure is the most direct ARDE control knob:

```
Strategy: Use low pressure during etch to reduce ARDE

Baseline recipe (high ARDE):
  P = 100 mTorr, W = 2000 W, W_bias = 500 W
  ARDE ≈ 3.5 (severe)
  Etch rate uniformity: ±35% (unacceptable)

Modified recipe (ARDE compensation):
  P = 60 mTorr, W = 2000 W, W_bias = 500 W
  ARDE ≈ 1.8 (much better)
  Etch rate uniformity: ±15% (acceptable)
  Trade-off: Overall etch rate drops 15-20%

Two-step etch (optimal):
  Step 1: Etch to 80% completion at P = 60 mTorr
    - Fast (~70 nm/min), uniform ARDE compensation
    - Time: ~60 seconds
    - Depth achieved: ~3500 nm
  
  Step 2: Final 20% at P = 100 mTorr
    - Slower (~50 nm/min), better selectivity at end-point
    - Time: ~30 seconds
    - Total depth: ~4000 nm
  
  Result: Overall throughput better than single-pressure, ARDE well-controlled
```

### 10.3.2 RF Bias Power Modulation

Ion energy (controlled by bias power) affects ion penetration:

```
Mechanism:
  Higher E_ion → ions penetrate deeper into trench
  Lower E_ion → ions stopped in shallow region
  
Ion penetration depth (rough estimate):
  E_ion = 50 eV: penetration ≈ 500 nm (stopped near opening)
  E_ion = 80 eV: penetration ≈ 1000 nm (mid-depth)
  E_ion = 150 eV: penetration ≈ 2000+ nm (full depth)

ARDE control via W_bias:

Baseline (low bias, 200 W):
  E_ion ≈ 30 eV
  Ion penetration poor
  ARDE ≈ 4.0 (very severe)

Optimized (medium bias, 600 W):
  E_ion ≈ 80 eV
  Better ion penetration
  ARDE ≈ 2.0 (improved)

Aggressive (high bias, 1000 W):
  E_ion ≈ 150 eV
  Good penetration, but selectivity degrades
  ARDE ≈ 1.2 (low), but C/SiO₂ selectivity drops to 10:1 (risky)

Trade-off: High bias reduces ARDE but degrades selectivity.
Compromise: Mid-range bias (600-800 W) balances ARDE and selectivity.
```

### 10.3.3 Pulsed Plasma Etch (Time-Modulated)

Alternating etch/pause cycles can reduce ARDE:

```
Continuous etch (baseline):
  Plasma ON continuously for 120 seconds
  ARDE ≈ 3.5 (severe)
  Etch uniformity: ±35%

Pulsed etch (50% duty cycle):
  Pattern: 1 sec ON, 1 sec OFF, repeat
  Total time: 240 seconds (2× longer than continuous)
  
  Physical effect:
    During ON (1s): Fast etch at all depths
    During OFF (1s): Thermal equilibration, polymer annealing
    
    Polymer at deep regions:
      - Builds thicker during ON periods
      - Decomposes partly during OFF periods
      - Net polymer thickness reduced → less blocking effect
      - Deep trench etch rate improves
    
    Net result:
      ARDE ≈ 1.8 (much improved!)
      Etch uniformity: ±15% (acceptable)

Trade-off:
  Throughput reduced 2× (240s vs. 120s)
  Better uniformity justifies longer time for precision applications

Pulsing parameters:
  Duty cycle: 25%-75% (25% very slow, 75% little benefit)
  Optimal: 50%-60% duty cycle
  Pulse frequency: 0.5-2 Hz (avoid resonance with thermal time constant)
```

### 10.3.4 In-Situ Feedback Compensation

Real-time monitoring and correction:

```
Closed-loop ARDE compensation:

Measurement:
  - Endpoint detection sensor (optical or electrical)
  - Tracks etch progress at multiple radial positions
  - Center (region 1), Mid-radius (region 2), Edge (region 3)
  
Example measurement (time vs. endpoint signal):
  Region 1 (center): Reaches endpoint at t = 90 sec
  Region 2 (mid):    Reaches endpoint at t = 95 sec
  Region 3 (edge):   Reaches endpoint at t = 105 sec
  
  Variation: 105/90 = 1.17 (17% time variation due to ARDE)

Feedback correction:
  1. Detect when Region 1 hits endpoint (fast region)
  2. Reduce W_coil (main etch power) by 10%
  3. Maintain W_bias (selectivity tuning)
  4. Slower etch at top allows bottom to catch up
  5. Regions 2 and 3 continue at higher rate
  6. All regions reach endpoint within 2% (±1%)

Outcome:
  Etch uniformity: ±17% → ±2% (8.5× improvement!)
  Cost: Increased process complexity, sensor maintenance
  Benefit: Enables deep NAND patterns at high precision

Production implementation:
  ~10-15% of advanced fabs have in-situ feedback
  Growing adoption as NAND stacks reach 200+ layers
```

---

## 10.4 ARDE Effects on 3D NAND Geometry

### 10.4.1 Linewidth Variation from ARDE

ARDE directly impacts final linewidth:

```
Design intent: 15 nm linewidth throughout 5000 nm deep trench

With uncompensated ARDE (ARDE = 3.5):
  
  Opening (top):
    Time to etch 15 nm: t = 15 nm / 80 nm/min ≈ 0.19 min
    At this time, bottom etch rate: 80/3.5 ≈ 23 nm/min
    Bottom depth etched: 23 × 0.19 ≈ 4.4 nm (only 4.4 nm!)
  
  At 2500 nm depth (half etch):
    Etch rate: ~40 nm/min (between top and bottom)
    Linewidth at this depth: ~17-18 nm (slightly wider)
  
  At 5000 nm depth (bottom):
    Etch rate: ~23 nm/min (27% of top rate)
    Linewidth: ~25-30 nm (100%+ wider than target!)

Result:
  Intended feature: 15 nm
  Top of trench: 14-16 nm (±7%, acceptable)
  Mid-trench: 17-18 nm (±15%, concerning)
  Bottom: 25-30 nm (±80%, FAILING)
  
  Device fails: Gate stack at bottom is too wide, transistor behavior degraded
```

### 10.4.2 Pattern Distortion and Stress

ARDE creates non-uniform etching that distorts structures:

```
Cross-section of wide trench (500 nm wide):

Without ARDE (uniform):
  |╱╲|      Vertical sidewalls
  | ╲ \|    Width constant with depth
  |  \  \|  Pattern preserved
  |   \   \|
  └─────────┘

With severe ARDE:
  |╱╲╲╲╲╲╲╲╲╲╲╲╲╲|  Very narrow at opening (less time for wide erosion)
  | ╲  ╲      \ |  Tapered sidewalls
  |  \  \      \ |  Pattern distorted (narrower at top)
  |   \  \      \|  Creates mechanical stress
  |    \  \     ╱|  Trench width increases with depth
  └─────────────┘   (opposite expected direction)

Stress consequences:
  - Pattern narrowing at top can prevent lithography
  - Linewidth variation causes lithographic stops (layers with too-narrow gaps)
  - Mechanical stress from tapering can cause delamination
  - Device array becomes non-functional at certain depths

Production impact:
  Yield loss from ARDE-induced pattern failure: 5-15% per node generation
  Significant cost driver for fabs with insufficient ARDE compensation
```

---

## 10.5 ARDE Measurement and Characterization

### 10.5.1 Test Structures for ARDE Quantification

Fabs use special test patterns to measure and quantify ARDE:

```
ARDE test structure (cross-section):

  Carbon hard mask: 20 nm thick
  
  Features at different aspect ratios:
  
  AR=1:1    AR=10:1    AR=50:1    AR=100:1
  50nm      50nm       50nm       50nm
  deep      deep       deep       deep
  │         │          │          │
  ▼         ▼          ▼          ▼
  ╱╲        ╱╲╲╲╲╲╲╲╲╲╲╲╲╲╲╲╱╲╲╲╲╲╲╲╲╱
  
  All features etched simultaneously

Measurement procedure:
  1. Etch test pattern completely (carbon removed, substrate exposed)
  2. Cross-section TEM/SEM: Measure etch depth at each AR
  3. Calculate etch rate: R = depth / etch_time
  4. Calculate ARDE: ARDE = R_small_AR / R_large_AR
  
  Example results:
    AR=1:1: Depth = 50 nm (complete)
    AR=10:1: Depth = 50 nm (complete)
    AR=50:1: Depth = 40 nm (80% complete)
    AR=100:1: Depth = 20 nm (40% complete)
    
    ARDE (50:1 vs. 1:1) = 50/40 = 1.25 (acceptable)
    ARDE (100:1 vs. 1:1) = 50/20 = 2.5 (moderate)
```

### 10.5.2 OES Monitoring of ARDE

Optical emission spectroscopy can indicate ARDE effects:

```
Principle:
  F· radical emission at 703.7 nm indicates fluorine activity
  Polymer deposition (CF₂ species) appears as different spectral lines
  
  If ARDE is high (top etching fast, bottom slow):
    Early in etch: High F· emission (fast reaction at top)
    Late in etch: Lower F· emission (bottom becomes depleted, less reaction)
    Pattern: Emission drops over time as ARDE effect builds up

Example OES trend:
  
  F· intensity (703.7 nm)
      100% |╌╌╌╌╌╌╌╌╌╌╌╌
           |╱╱╱╱╱╱╱╱╱╱ (dropping, ARDE building)
       75% |╱╱╱╱╱╱
           |╱╱╱╱
       50% |╱╱
           |╱
       25% |
      Time ─────────
           0s   60s  120s
      
  Declining trend: Indicates ARDE accumulation
  Flat trend: Indicates good ARDE compensation

Use as feedback:
  Monitor F· emission trend
  If slope too steep: Increase pressure or bias power (improve deep etch)
  If flat: Recipe is well-balanced for ARDE compensation
```

---

## 10.6 ARDE Compensation in Production Recipes

### 10.6.1 Multi-Step Etch Strategy

Most production recipes use multiple steps to handle ARDE:

```
Example: Etch 5000 nm trench in carbon (100:1 AR)

Single-step (naive, fails due to ARDE):
  Step 1: P=100 mTorr, W=2000W, W_bias=500W, Duration=120s
  Result: Top half complete (4000 nm), bottom 1000 nm underetched
  Status: FAIL

Three-step recipe (optimized):

  Step 1: Fast etch (top 60%)
    Pressure: 60 mTorr (low, for penetration)
    Coil power: 2000 W
    Bias power: 600 W (good ion penetration)
    Duration: 60 seconds
    Depth: ~3000 nm
    ARDE: Moderate due to low pressure
    Selectivity: Moderate (~12:1), acceptable for bulk etch
  
  Step 2: Medium etch (next 30%)
    Pressure: 80 mTorr (rising, as depth increases)
    Coil power: 1800 W (slight reduction)
    Bias power: 500 W (selectivity tuning)
    Duration: 45 seconds
    Depth: ~1500 nm more (total 4500 nm)
    ARDE: Lower (more ion-assisted from higher bias)
    Selectivity: Better (~15:1)
  
  Step 3: Final polish (last 10%)
    Pressure: 100 mTorr (higher for selectivity)
    Coil power: 1000 W (much lower, slow etch)
    Bias power: 400 W (reducing ion energy)
    Duration: 30 seconds
    Depth: ~500 nm (reaches full 5000 nm)
    ARDE: Minimal (already at depth)
    Selectivity: Excellent (~20:1), protects underlayer
  
  Total time: 135 seconds (vs. 120s single-step)
  Etch uniformity: ±8% across all depths (acceptable)
  Selectivity: Protected at end-point (good for device integration)
```

### 10.6.2 Endpoint Detection Challenges with ARDE

Different regions reach endpoint at different times:

```
Three radial zones monitored:

Zone A (center):    Fast etch, reaches endpoint at 90s
Zone B (mid):       Medium etch, reaches endpoint at 100s
Zone C (edge):      Slow etch, reaches endpoint at 120s

Challenge: Which region to use as endpoint signal?

Option 1: Use fastest region (Zone A at 90s)
  Problem: Zones B and C still being etched
  Result: Over-etch in B and C → wider linewidth at edges (yield loss)

Option 2: Use slowest region (Zone C at 120s)
  Problem: Zone A over-etched by 30 seconds
  Result: Over-etch in A → narrow linewidth at center
  Also risks selectivity loss at edges due to excessive etch

Option 3: Use weighted average (stop at 105s, compromise)
  Zone A: 15 seconds over-etch
  Zone B: 5 seconds over-etch
  Zone C: 15 seconds under-etch
  Result: Etch uniformity ~±15% (acceptable compromise)

Best practice:
  Monitor multiple radial points
  Use in-situ feedback to adjust power, slowing down as fast regions complete
  Achieve all zones within ±2% (difficult, requires good ARDE compensation)
```

---

## 10.7 Summary & Key Takeaways

1. **ARDE Severe in High-AR Trenches**: 100:1 aspect ratios create 5-10× etch rate variation (top vs. bottom)

2. **Three Coupled Mechanisms**: Ion depletion (primary, 70-80% effect), neutral shadowing (10-15%), polymer redeposition (10-15%)

3. **Pressure is Primary Knob**: Low pressure (60 mTorr) vs. high (100 mTorr) changes ARDE 2-3×; lower pressure enables deep penetration

4. **Multi-Step Recipes Essential**: Three-step etch (fast/medium/slow) achieves ±8% uniformity vs. ±35% for single-step

5. **Ion Energy Control Critical**: Higher bias power improves ion penetration; trade-off with selectivity (need balance)

6. **Pulsed Plasma Helps**: 50% duty cycle reduces ARDE 2× by allowing polymer annealing, but increases process time

7. **In-Situ Feedback Powerful**: Real-time endpoint monitoring and power adjustment can achieve ±2% uniformity vs. ±15% open-loop

8. **Linewidth Distortion Real**: Uncompensated ARDE creates 80-100% linewidth variation top to bottom, catastrophic for device function

---

## Study Questions

1. Calculate ARDE for 50 nm wide trench 2500 nm deep, with top etch rate 80 nm/min and bottom 20 nm/min.

2. Explain how lower pressure (50 mTorr) reduces ARDE compared to higher pressure (150 mTorr).

3. Design three-step etch recipe for 5000 nm trench: suggest pressure, power, and duration for each step.

4. Predict linewidth at trench bottom if top linewidth is 15 nm and ARDE = 3.5. How bad is this deviation?

5. Describe pulsed etch (duty cycle, frequency) to reduce ARDE. What is the throughput trade-off?

---

**Next Chapter:** [Chapter 11: Ion Energy & Ion Flux Distribution Control](./11-ion-energy-control.md)

---

**Chapter 10 Development Status:** Comprehensive content complete  
**Last Updated:** October 3, 2026  
**Version:** 1.0 (Complete technical chapter)
