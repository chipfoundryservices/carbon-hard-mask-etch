# Chapter 6: Gas Distribution & Radical Uniformity in High-Aspect-Ratio Structures

## Overview

Fluorine radical (F·) concentration uniformity across the wafer directly determines etch rate uniformity. Unlike thermal management (which is passive heat spreading), gas distribution is an active flow problem requiring careful showerhead design, pressure control, and validation. The challenge intensifies for 3D NAND: radicals must penetrate deep (2000+ nm) into high-aspect-ratio trenches (100:1+) while maintaining uniform concentration across the wafer surface.

**Learning Objectives:**
- Design showerhead gas delivery systems for 300mm uniformity
- Understand computational fluid dynamics (CFD) modeling of gas flow
- Analyze radical transport into high-aspect-ratio features
- Quantify mean free path and collision-dominated vs. ballistic regimes
- Implement in-situ measurement of radical uniformity
- Optimize pressure and gas flow for deep trench penetration

---

## 6.1 Showerhead Design Fundamentals

### 6.1.1 Showerhead Architecture

Showerheads distribute gases uniformly from chamber inlet to wafer surface:

**Basic showerhead geometry:**

```
Gas inlet
    ↓
┌─────────────────┐
│ Distribution    │
│ cavity (low P)  │  Pressurized volume for velocity reduction
└─────────────────┘
    ↓ (slow flow through orifices)
╔═════════════════╗
║ Orifice array   ║  Matrix of small holes (0.5-2 mm diameter)
║   ·   ·   ·     ║  Geometry and spacing control velocity
║   ·   ·   ·     ║  
║   ·   ·   ·     ║
╚═════════════════╝
    ↓
    ↓
   ╱╲╱╲╱╲╱╲╱╲    Gas plume/mixing region
  ╱  ╱  ╱  ╱  
 ───────────────  Wafer surface (~20-100 mm away)
```

**Showerhead types:**

| Type | Geometry | Flow control | Uniformity | Maintenance |
|------|----------|-------------|-----------|-------------|
| Packed hole | Grid of ~0.5-1mm holes | Pressure-based | Moderate (±10%) | High (carbon buildup) |
| Slit jet | Linear slots or radial slots | Slot width/length control | Good (±5%) | Moderate |
| Multi-ring | Concentric rings of orifices | Ring pressure control | Excellent (±2%) | Low |
| Hybrid | Combination of above | Multiple control points | Best (±1-2%) | Moderate |

### 6.1.2 Orifice Sizing and Spacing

Orifice design determines gas jet uniformity:

**Design parameters:**

```
Packed orifice showerhead (most common):

Orifice diameter: d_orifice = 0.5-2.0 mm
Typical choice: d = 1.0 mm for 300mm chamber

Number of orifices:
  For uniform coverage: N_orifices ≈ π × (D/pitch)²
  
  300mm chamber, 10mm pitch: N ≈ π × (300/10)² ≈ 2800 orifices
  
  In practice: 1000-3000 orifices (complex array)

Orifice spacing:
  Pitch (distance between orifice centers): 8-15 mm
  Typical: 10 mm pitch (balance of uniformity and mechanical complexity)

Aspect ratio (hole depth to diameter):
  d_orifice = 1.0 mm
  Hole depth: 3-5 mm (long, thin holes)
  Aspect ratio: 3-5:1
  
  Deeper holes (higher aspect ratio):
    → More pressure drop across orifice
    → Better uniformity (pressure-driven, less velocity variation)
    → Drawback: Pressure drop reduces flow capacity
```

**Velocity at orifice (from pressure drop):**

```
Choked flow (if pressure drop is large enough):
  V_orifice = √(2 × γ × R × T / (γ + 1)) × √(1 - (P_down/P_up)^((γ-1)/γ))
  
For most gas etch systems:
  - Upstream pressure (inlet): P_inlet = 5-10 psi (gauge)
  - Downstream pressure (chamber): P_chamber = 50-200 mTorr = 0.0007-0.003 psi
  - Large ratio: P_inlet / P_chamber >> 1
  
  Orifice flow is nearly sonic (Mach ~0.8-1.0)
  Velocity: V_orifice ≈ 200-400 m/s (high speed jet)
  
Exit velocity from orifice is independent of downstream pressure (sonic limit).
This provides excellent flow control.
```

### 6.1.3 Gas Distribution Chamber

Upstream of orifices, pressure regulation is critical:

**Distribution chamber function:**

```
Inlet manifold → [High pressure region, P = 5-10 psi]
                        ↓
                  [Distribution chamber]
                   - Volume: ~5-20 liters
                   - Purpose: Reduce velocity, increase pressure uniformity
                        ↓
                  [Orifice array]
                   - Each orifice sees same upstream P
                   - Flow through each orifice: Q = (P_upstream) / R_orifice
                        ↓
                  Uniform gas distribution (ideally)
```

**Uniformity in distribution chamber:**

Pressure uniformity across distribution chamber is critical:

```
If distribution chamber volume is small:
  → Slow pressure equilibration
  → Pressure variation during transient gas changes
  → Localized orifice flow variations
  
If volume is large:
  → Good pressure uniformity
  → Fast recovery from transient perturbations
  
Optimal design:
  Volume / inlet flow = 10-30 seconds (chamber "time constant")
  
  For typical flow rate (50-200 sccm = ~0.03-0.12 liters/min):
  Volume ≈ 0.3-1.2 liters (small, achievable in 3-6" chamber height)
```

---

## 6.2 Computational Fluid Dynamics (CFD) and Gas Flow Simulation

### 6.2.1 CFD Modeling Approach

Modern showerhead design uses CFD simulation to optimize gas distribution:

**CFD domain:**

```
Computational domain (example 300mm chamber):

   ┌─────────────────────────────────────────┐
   │ Gas inlet (top, 10mm diameter)          │
   │ Inlet velocity: 5 m/s (typical)         │
   │ Inlet temperature: 20°C                 │
   │ Inlet gas: CF₄/Ar mix                   │
   └──────┬──────────────────────────┬───────┘
          │ Showerhead (3D orifice)  │
          ├─ 2800 orifices            │
          ├─ 1mm diameter each        │
          │ Orifice velocity: ~300 m/s│
          └─────────┬──────────────────┘
                    │
                    │ Jet mixing region
                    │ ~50mm free space
                    │
           ╭────────┴──────────┬─────────╮
           │                   │         │
        Sidewall walls      Wafer      Chamber wall
        (adiabatic)      (temp sink)  (adiabatic)
```

**Grid and physics:**

```
Mesh resolution:
  - Showerhead region: Fine mesh (~0.1-0.5 mm elements)
  - Mixing region: Moderate mesh (~1-2 mm)
  - Far field (near walls): Coarse mesh (~5-10 mm)
  
  Total elements: 1-10 million (depending on detail level)

Physics modeled:
  - Continuity equation: ∇·(ρu) = 0 (mass conservation)
  - Momentum: ρ(u·∇u) = -∇P + ∇·τ + ρg (Navier-Stokes)
  - Energy: ρCp(u·∇T) = ∇·(k∇T) (heat transfer)
  - Species: ∇·(ρuCi) = ∇·(ρDi∇Ci) (radical transport)
  - Turbulence: k-ε or k-ω model (for Re > 10,000 jets)

Boundary conditions:
  - Inlet: Specified flow rate and temperature
  - Outlet (chamber side): Pressure specified (50-200 mTorr)
  - Wafer surface: Velocity = 0 (no-slip), sink for gas absorption
  - Walls: Adiabatic (no heat transfer), no-slip
```

### 6.2.2 CFD Results Interpretation

Typical CFD output for 300mm chamber:

**Pressure field:**

```
Pressure distribution (mTorr) - contour map:

Showerhead:        200 mTorr (high pressure region)
Jets (initial):    150-180 mTorr
Jet mixing:        100-150 mTorr
Far field:         50-80 mTorr (chamber pressure)
Wafer surface:     ~50 mTorr (average, small variation ±10%)

Pressure uniformity at wafer surface: ±20% variation (typical)
This is acceptable (gas distribution is inherently non-uniform).
```

**Velocity field:**

```
Velocity magnitude (m/s) - vector field:

Showerheads jets:      ~300 m/s (supersonic-like)
Jet expansion zone:    100-200 m/s (spreading)
Mixing region:         10-50 m/s (decaying jets)
Wafer surface:         <1 m/s (near-stagnant flow)

Streamlines show: Jets from center showerhead reach wafer center with minimal
spreading; edge orifices create jets that spread more widely.

Edge effects: Gases tend to accumulate at wafer edges (pressure higher there).
```

**Radical concentration field:**

```
Simulated F· concentration (arbitrary units, assuming uniform inlet):

Showerhead:         100% (inlet concentration)
Jet core:           80-90% (minor mixing)
Jet edges:          50-80% (mixing with chamber gas)
Wafer surface:
  - Center:         60-70% (high concentration from central jets)
  - Mid-radius:     50-60% (reduced from spreading jets)
  - Edge:           40-50% (diluted by edge recycling)

Radial uniformity: ±30% variation (center to edge)

This non-uniformity translates to ±30% etch rate variation without compensation.
```

### 6.2.3 CFD Optimization Workflow

Iterative design process to improve uniformity:

```
1. Initial design
   ├─ Standard showerhead (packed orifices)
   ├─ CFD simulation
   └─ Results: ±30% pressure, ±30% radical concentration
   
2. Identify problem areas
   ├─ Edge concentration too high → jets over-focused
   ├─ Center concentration slightly low → jets too spread out
   └─ Asymmetry → orifice blockage or manufacturing error
   
3. Design modification
   ├─ Option A: Adjust orifice spacing (finer at edge, coarser at center)
   ├─ Option B: Add baffle plates to redirect jets
   ├─ Option C: Modify distribution chamber (asymmetric inlets)
   └─ Option D: Change orifice diameter variation
   
4. Re-simulate
   └─ New CFD results: ±15% pressure, ±20% radical concentration
   
5. Iterate until
   ├─ Pressure uniformity: ±5%
   ├─ Radical uniformity: ±10% (achievable goal)
   └─ Manufacturable design (no over-complexity)
   
6. Experimental validation
   ├─ Build physical showerhead
   ├─ Measure in chamber (optical diagnostics)
   ├─ Compare to CFD predictions
   └─ Adjust model if needed
```

---

## 6.3 Radical Transport into High-Aspect-Ratio Trenches

### 6.3.1 Mean Free Path and Transport Regimes

Radical penetration into trenches depends on mean free path (MFP):

**Mean free path calculation:**

```
MFP = 1 / (√2 × n × σ)

where:
  n = number density of gas molecules
  σ = collision cross-section (~5-10 Å² for fluorine)

For fluorine radicals in chamber:

At 50 mTorr, 20°C:
  n = P / (k_B × T) = (50 mTorr × 133 Pa/mTorr) / (1.38×10⁻²³ J/K × 293 K)
    = 6.7 / 133 Pa / (1.38×10⁻²³ × 293)
    = 1.7 × 10¹⁹ molecules/m³
  
  MFP_F = 1 / (√2 × 1.7×10¹⁹ × 8×10⁻²⁰) ≈ 5 μm

At 100 mTorr:
  n = 2× higher
  MFP ≈ 2.5 μm

At 200 mTorr:
  MFP ≈ 1.2 μm
```

**Transport regimes:**

```
Free molecular (ballistic) flow: MFP >> Trench dimension
  MFP ≈ 5 μm, Trench width ≈ 50 nm → Ratio = 100:1 (ballistic)
  Radicals travel straight, collisions rare
  Radical concentration predictable from geometry alone
  
Collision-dominated (viscous) flow: MFP << Trench dimension
  MFP ≈ 1 μm, Trench width ≈ 50 nm → Ratio = 20:1 (intermediate)
  Mix of ballistic and diffusive transport
  Neutral species spread by diffusion
  
Hydrodynamic flow: MFP << Trench dimension
  MFP ≈ 100 nm, Trench width ≈ 50 nm (unusual, high pressure)
  Continuous flow, no-slip at walls
  Poiseuille flow profile in trenches
```

**Most 3D NAND etch operates in intermediate regime** (MFP ~ trench width).

### 6.3.2 Aspect Ratio Effects on Radical Penetration

Penetration into deep trenches is limited by geometry:

**Ion penetration model:**

```
Trench geometry:
  Width: w = 50 nm
  Depth: d = 5000 nm (100:1 aspect ratio)
  Aspect ratio: AR = d/w = 100:1

Radical flux at trench opening (wafer surface):
  Φ_opening = n_F · v_F / 4
  
  where v_F = √(8kT / πm_F) = thermal velocity of F· radicals
  At 20°C: v_F ≈ 800 m/s
  
  Φ_opening ≈ 10¹⁹ × 800 / 4 ≈ 2×10²¹ radicals/(m²·s)

Radical flux at trench depth (d = 5000 nm):
  Reduction due to:
  1. Molecular shadowing: Width of trench blocks radicals from side
  2. Consumption: F· consumed at top and sides before reaching bottom
  3. Sticking: Some F· sticks to sidewalls, doesn't reach bottom
  
  Combined effect:
  
  Φ_depth / Φ_opening = exp(-2 × d / w × sticking_coefficient)
                      = exp(-2 × 100 × 0.5)
                      = exp(-100)
                      ≈ 10⁻⁴⁴ (essentially zero)
```

This extreme suppression is why etch rate in deep trenches is so low (ARDE).

### 6.3.3 Pressure Optimization for Deep Trench Penetration

Pressure is a critical knob for controlling radical penetration:

**Effect of pressure on penetration:**

```
At LOW pressure (10 mTorr):
  MFP ≈ 50 μm (very long)
  Radicals are ballistic (straight-line travel)
  Penetrate deep into trenches
  Etch rate more uniform across depth
  Drawback: Low collision frequency → low plasma density → low etch rate overall

At MEDIUM pressure (50-100 mTorr):
  MFP ≈ 2-5 μm (comparable to trench features)
  Mix of ballistic and diffusive transport
  Good etch rate + reasonable uniformity trade-off
  PRODUCTION STANDARD

At HIGH pressure (200+ mTorr):
  MFP ≈ 0.5-1 μm (very short, diffusive)
  Radicals diffuse more than travel straight
  Poor penetration to deep trenches
  High etch rate but severe ARDE (top faster than bottom)
```

**Etch rate vs. pressure:**

```
Etch rate (nm/min)
  100 |           ╱╲___
      |        ╱╱    ╲___
   75 |     ╱╱          ╲___
      |  ╱╱                ╲___
   50 |╱╱
      |
   25 |
      |
    0 |_________________
      0   50  100  150  200  250  P (mTorr)

Peak etch rate: ~80-100 nm/min at 50-75 mTorr
At higher P: Rate drops due to lower dissociation efficiency
At lower P: Rate lower due to fewer collisions generating radicals

Production recipes typically operate at 50-100 mTorr (peak efficiency region).
```

---

## 6.4 Measurement and Diagnostics of Gas Distribution

### 6.4.1 In-Situ Optical Emission Spectroscopy (OES)

OES measures gas composition during operation:

**Principle:**

```
Plasma emits light from excited species:
  - F atom (excited): emits at 703.7 nm (red), 763.9 nm (red)
  - CF radical: emits UV/visible
  - Ar (excited): emits at 750-850 nm (near-IR)

By measuring emission intensity at these wavelengths, we infer:
  - Fluorine concentration (from F line intensity)
  - Plasma temperature (from line broadening)
  - Gas uniformity (by measuring radially)
```

**Measurement setup:**

```
Spectrometer positioned outside chamber with fiber optic probe:
  
  Chamber ────[Optical window]────[Fiber optic probe]────[Spectrometer]
  
  Spectrometer scans: 200-1000 nm wavelength range
  Resolution: 0.1-0.5 nm (sufficient for atomic lines)
  Integration time: 1-10 seconds (captures plasma variations)
```

**Spatial uniformity measurement:**

```
Radial scanning:

Spectrometer measures along three radii (0°, 120°, 240°):
  - Center (r = 0): Baseline intensity
  - Mid-radius (r = 75mm): Intensity check
  - Edge (r = 150mm): Intensity check
  
Example results (F 703.7 nm line intensity):

Radius (mm)   Intensity (a.u.)   Relative (%)
    0             10000           100%
   75             8500             85%
  150             7000             70%

Radial uniformity: Center to edge variation = 30% (typical)
```

### 6.4.2 Non-Optical Diagnostics: Mass Spectrometry

Mass spectrometry identifies gas species composition:

**Principle:**

```
Measure mass distribution of ions/radicals in chamber:

Plasma ──[Ion extraction]──[Mass filter]──[Detector]──[Data]

Quadrupole mass spectrometer (typical):
  - Extracts ions through small orifice
  - Applies time-varying voltages to pass specific masses
  - Detects arrival at electron multiplier
  - Provides abundance vs. mass ratio
```

**Typical species measured:**

```
Mass (amu)  Species      Abundance (%)*
   2        H₂           <1%
   4        He           <0.1%
  18        H₂O          ~2% (outgassing)
  20        Ar⁺          ~30%
  31        CF⁺          ~20%
  50        CF₂⁺         ~10%
  69        CF₃⁺         ~5%
  88        CF₄⁺         ~3%

*Approximate, depends on gas mixture and plasma conditions
```

### 6.4.3 Etch Uniformity Verification

Most direct measure: Etch wafers and measure uniformity:

**Test structure approach:**

```
Test wafer with uniform carbon hard mask (20 nm):

Process:
  1. Etch test wafer through full carbon thickness (complete etch)
  2. Measure remaining carbon thickness at multiple locations
     (by ellipsometry or XRF)
  3. Construct radial uniformity map
  4. Calculate uniformity: σ / μ (std dev / mean)

Target: ≤±5% etch rate uniformity (achievable with good showerhead)

Typical results:
  - Center etch time: T_center (carbon completely removed)
  - Edge etch time: T_edge (carbon completely removed)
  - Difference: ΔT / T_avg ≈ ±10% (without compensation)
```

---

## 6.5 Gas Flow Transient Behavior

### 6.5.1 Response to Gas Flow Changes

When recipe changes gas composition, chamber response has time constant:

```
Example: Switch from CF₄/Ar (50/50) to C₂F₆/Ar (30/70)

Chamber response:

F· concentration
  100% |────╌╌╌╌  (CF₄ baseline)
       |
   75% |         ╲
       |          ╲
   50% |           ╲╲___
       |              ╲╲___
   25% |                  ╲╌╌╌  (C₂F₆ new steady-state)
       |
    0% |_________________
       0    10   20   30   60  Time (seconds)

Transient behavior:
  - Initial drop: ~1 second (fast exchange at showerhead)
  - Exponential decay: ~30-60 seconds (filling/mixing chamber volume)
  - Steady-state: After ~3 chamber volume exchanges

Chamber time constant: τ ≈ V_chamber / Q_gas
  For typical chamber (100 L) and flow (100 sccm):
  τ = 100 L / (100 sccm / 60) = 60 seconds
```

**Production consequence:**

Wafers processed immediately after gas change see different etch rate:

```
Wafer 1 (immediately after change): Etches at 50% target rate (transition)
Wafer 2: Etches at 70% target rate (still transient)
Wafer 3: Etches at 85% target rate
Wafer 4+: Etch at 100% target rate (equilibrium)

Solution: Skip 2-3 wafers after gas change, or run "dummy" (discardable) wafers
```

### 6.5.2 Temperature Effects on Gas Flow

Temperature affects gas density and viscosity:

```
Ideal gas law: P = n k_B T = (ρ / M) R T

Effect of temperature on gas density:
  ρ ∝ 1/T (inverse proportional)
  
If electrode heats up: T rises 30°C (20°C → 50°C)
  ρ_new / ρ_old = T_old / T_new = 293 K / 323 K ≈ 0.91
  Density drops ~9%
  
This reduces F· concentration (fewer molecules per unit volume) by ~9%
Etch rate drops ~5-9% (density-dependent part of chemical etch)

Viscosity effect:
  μ ∝ √T (Sutherland's law approximation)
  
  μ_new / μ_old = √(323 / 293) ≈ 1.05
  Viscosity increases ~5%
  This increases flow resistance, reducing effective flow rate slightly
  
Combined effect: 10-15% reduction in effective radical flux if temperature rises 30°C
```

**Production mitigation:**

Temperature control (±2°C) ensures consistent gas flow characteristics.

---

## 6.6 Chamber Pressure Stability

### 6.6.1 Pressure Control Loop

Chamber pressure is maintained by balancing inlet flow and outlet pump:

```
Gas inlet (Q_in, controlled by mass flow controller)
    ↓
[Chamber] ← Pressure sensor monitors
    ↓
Throttle valve (controls pump conductance C_pump)
    ↓
Vacuum pump

Steady-state:
  Q_in = P_chamber × C_pump (at equilibrium)
  
  Therefore:
  P_chamber = Q_in / C_pump (proportional to inlet flow)
```

**Pressure stability:**

```
Factors affecting stability:

1. Mass flow controller accuracy: ±2-5% typical
   If set to 100 sccm, actual may be 95-105 sccm
   Causes pressure variation: ±2-5%

2. Throttle valve drift: ~±1-2% per day (thermal effects)
   Requires active feedback control

3. Pump performance variation: ~±3-5% with age, fouling
   Pump speed degrades over time

Combined uncertainty: ±5-10% pressure variation (typical)

Production target: ±5% pressure variation achievable with good control
```

---

## 6.7 Summary & Key Takeaways

1. **Showerhead Design Drives Uniformity**: Packed orifices can achieve ±10-20% radical uniformity; multi-ring designs achieve ±5%

2. **CFD Simulation is Essential**: CFD predicts pressure and radical fields; experimental validation required but CFD dramatically reduces trial-and-error

3. **Mean Free Path Controls Penetration**: At process pressures (50-100 mTorr), MFP ~2-5 μm; comparable to feature sizes, making penetration limited

4. **Pressure is Critical Knob**: 50-100 mTorr optimal for etch rate + uniformity trade-off; lower P better for deep trench access, higher P better for overall rate

5. **Transient Response is Slow**: Chamber time constant ~30-60 seconds; gas composition changes require 1-2 minute equilibration

6. **Temperature Affects Flow**: 30°C temperature rise reduces F· concentration ~10%; ±2°C control ensures stable gas delivery

7. **Etch Uniformity ±5-10%**: Achievable with optimized showerhead + proper pressure control; residual non-uniformity compensated by process tuning

---

## Study Questions

1. Design a packed-orifice showerhead for 300mm chamber: What orifice diameter, spacing, and array density would you choose? Why?

2. Calculate mean free path of F· radicals at 75 mTorr, 25°C. Compare to 50 nm trench width. What transport regime dominates?

3. Interpret CFD results: If simulated F· concentration shows ±30% variation (center to edge), what design changes might improve uniformity to ±10%?

4. Explain why etch rate varies when recipe changes gas from CF₄/Ar to C₂F₆/Ar. How long does the transition take?

5. Design an OES diagnostic: What wavelengths would you measure to detect (a) F atom concentration, (b) plasma temperature, (c) radial uniformity?

---

## References & Further Reading

- Lieberman, M.A., Lichtenberg, A.J., "Principles of Plasma Discharges and Materials Processing," 2nd ed., Wiley, 2005
- Bird, R.B., Stewart, W.E., Lightfoot, E.N., "Transport Phenomena," 2nd ed., Wiley, 2002 (fluid mechanics)
- Coburn, J.W., "Plasma-Assisted Etching," Applied Physics Letters, Vol. 42, 1983

**Next Chapter:** [Chapter 7: Pressure-Power-Temperature Phase Space for Carbon Mask Etch](./07-pressure-power-temp.md)

---

**Chapter 6 Development Status:** Comprehensive content complete  
**Last Updated:** October 3, 2026  
**Version:** 1.0 (Complete technical chapter)
