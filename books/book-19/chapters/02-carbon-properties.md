# Chapter 2: Carbon Material Properties & Plasma Interactions

## Overview

Carbon is one of the most versatile materials in semiconductor manufacturing, yet its behavior in plasma environments remains complex and node-dependent. This chapter establishes the material science foundation for carbon hard mask etching—the physical and chemical properties that drive etch behavior, selectivity, and integration challenges.

**Learning Objectives:**
- Understand amorphous carbon structure (sp² vs. sp³ bonding) and implications for plasma interactions
- Quantify thermal properties and thermal management requirements
- Analyze oxidation kinetics and surface chemistry evolution during etch
- Differentiate carbon variants and their effects on etch behavior
- Connect material properties to chamber design constraints

---

## 2.1 Amorphous Carbon Structure & Bonding

### 2.1.1 sp² vs. sp³ Bonding Fractions

Carbon can form three distinct bonding geometries, each with different properties:

**sp³ Hybridization (Tetrahedral):**
- Four covalent bonds at 109.5° angles
- Structure: Diamond-like, highly ordered
- Bond strength: 348 kJ/mol (C-C, very strong)
- Reactivity: Low (stable, hard to etch)
- Appearance: Colorless, transparent

**sp² Hybridization (Trigonal Planar):**
- Three covalent bonds at 120° angles; one π orbital
- Structure: Graphite-like layers, mobile electrons
- Bond strength: 265 kJ/mol (C=C)
- Reactivity: Higher (more reactive with plasma)
- Appearance: Dark, conductive

**sp¹ Hybridization (Linear, rare):**
- Two covalent bonds at 180° angles
- Structure: Acetylene-like (very rare in bulk)
- Not discussed further; negligible in amorphous carbon films

### 2.1.2 Amorphous Carbon Film Composition

Hard mask carbon films deposited by CVD are **not pure diamond (all sp³)** nor **pure graphite (all sp²)**. Instead, they are amorphous mixtures:

**Typical composition:**
- sp³ fraction: 40-80% (depends on deposition temperature and precursor)
- sp² fraction: 20-60%
- Hydrogen content: 0-5% (if using hydrocarbon precursors)

**Structure model:**
```
Amorphous Carbon Network:

    ╔═══════════════════════════════╗
    ║  Mixed sp² and sp³ regions    ║
    ║  ╭─────────────────────────╮  ║
    ║  │ Graphite-like domains   │  ║
    ║  │ (sp²-rich clusters)     │  ║
    ║  ╰─────────────────────────╯  ║
    ║           embedded in          ║
    ║  ╭─────────────────────────╮  ║
    ║  │ Diamond-like matrix     │  ║
    ║  │ (sp³-rich background)   │  ║
    ║  ╰─────────────────────────╯  ║
    ╚═══════════════════════════════╝

Grain size: <10 nm (no large crystals)
Structure: Continuous, no grain boundaries
```

### 2.1.3 Implications for Plasma Etch

The sp²/sp³ ratio directly affects etch behavior:

| Property | High sp³ (Diamond-like) | High sp² (Graphite-like) |
|----------|------------------------|--------------------------|
| Etch rate | Lower (stronger bonds) | Higher (weaker π bonds) |
| Plasma reactivity | Lower | Higher |
| Electrical conductivity | <10⁻¹⁵ S/cm | >10⁻³ S/cm |
| Charge buildup during etch | High | Low |
| Surface roughness evolution | Smoother | Rougher |
| Selectivity vs. SiO₂ | Better (higher etch rate difference) | Moderate |

**Production consequence:** Films with higher sp² content etch faster but create more charge buildup during plasma exposure, potentially causing electrostatic damage. Films with higher sp³ content are more stable but etch slower and require higher power.

---

## 2.2 Thermal Properties

### 2.2.1 Thermal Conductivity: The Critical Design Constraint

Thermal conductivity (k) is perhaps the most important material property for carbon hard mask etch chamber design.

**Typical values for various materials:**

| Material | Thermal Conductivity | Ratio to SiO₂ |
|----------|---------------------|---------------|
| Silicon (crystalline) | 150 W/m·K | 107:1 |
| Aluminum | 237 W/m·K | 169:1 |
| Copper | 385 W/m·K | 275:1 |
| Carbon (soft, CVD) | 20-50 W/m·K | 14-36:1 |
| Carbon (hard, deposited) | 50-100 W/m·K | 36-71:1 |
| SiO₂ (fused) | 1.4 W/m·K | 1:1 (reference) |
| Si₃N₄ | 2.0-3.0 W/m·K | 1.4-2.1:1 |

**Key insight:** Carbon's thermal conductivity is 100-500× *higher* than oxide/nitride but 2-5× *lower* than silicon/aluminum. This creates a critical design challenge:

- Carbon cannot rely on bulk heat spreading into substrate (too slow)
- Carbon cannot be passively cooled by radiation (too low surface area in chamber)
- **Active thermal management is mandatory** (not optional)

### 2.2.2 Temperature Uniformity Requirements

During high-current-density carbon etch plasma:

**Power density at wafer surface:**
- Local power: 1-3 W/cm² (typical)
- Peak local power (under hot spots): 5-10 W/cm² (non-uniform)
- Total wafer power dissipation: 500-2000 W for 300mm wafer

**Resulting temperature rise:**
- Initial wafer temperature: 20°C (ambient)
- Temperature rise during plasma: 30-50°C
- Target wafer temperature: -10°C to +60°C (production range)
- **Thermal uniformity requirement: ±2-3°C across 300mm diameter**

Why this tolerance is critical:

Etch rate has strong temperature dependence (Arrhenius relation):
```
R_etch = R₀ × exp(-E_a / kT)

where:
  R_etch = etch rate (nm/min)
  R₀ = pre-exponential factor
  E_a = activation energy (~10-20 kcal/mol for carbon/fluorine)
  k = Boltzmann constant
  T = temperature (K)
```

**Temperature sensitivity example:**
```
At 30°C: R_etch = 50 nm/min
At 35°C: R_etch = 57 nm/min (14% increase)
At 40°C: R_etch = 65 nm/min (30% increase per 10°C)
```

A 5°C temperature gradient across the wafer translates to ~±15% etch rate variation, which directly impacts linewidth uniformity and ARDE control.

### 2.2.3 Coefficient of Thermal Expansion (CTE)

CTE describes dimensional change with temperature:

**CTE for carbon and related materials:**

| Material | CTE (ppm/°C) | Note |
|----------|-------------|------|
| Carbon (amorphous) | 3-8 | Depends on sp²/sp³ ratio |
| Silicon | 2.6 | For comparison |
| SiO₂ | 0.5 | Significant mismatch! |
| Si₃N₄ | 3.0 | Similar to carbon |
| Aluminum | 23.1 | Very high expansion |

**CTE mismatch consequences:**

When carbon film (CTE ~5 ppm/°C) is deposited on SiO₂ substrate (CTE ~0.5 ppm/°C):

```
At deposition (300°C): Stress = 0 (same temperature)
Cool to 20°C: Substrate contracts more than film

Temperature change: ΔT = 300 - 20 = 280°C
CTE difference: Δ(CTE) = 5 - 0.5 = 4.5 ppm/°C

Strain: ε = Δ(CTE) × ΔT = 4.5 × 280 = 1260 μstrain

This creates residual compressive stress in the film (~500-1000 MPa typical)
```

**Implications during etch:**

- **Temperature rise during plasma**: Wafer heats 30-50°C
- **Thermal stress relief**: Carbon film expands more than substrate
- **Result**: Mechanical stress cycling that can cause:
  - Film cracking or delamination
  - Pattern distortion (linewidth variation)
  - Adhesion loss at film-substrate interface

Production consequence: Thermal transients during recipe changes must be managed carefully to avoid mechanical damage.

---

## 2.3 Mechanical Properties

### 2.3.1 Hardness & Elastic Properties

**Amorphous carbon mechanical properties:**

| Property | Typical Value | Context |
|----------|---------------|---------|
| Hardness (Vickers) | 500-3000 HV | Depends on sp³ fraction |
| Young's Modulus | 100-300 GPa | Hard films more elastic |
| Fracture Toughness | 1-5 MPa·√m | Brittle material |
| Tensile Strength | 1-5 GPa | High stress tolerance |
| Internal Stress (as-deposited) | -500 to -1500 MPa | Compressive (negative) |

**Hardness variation with sp³ content:**
- High sp³ (diamond-like): 2000-3000 HV (very hard)
- Medium sp²/sp³ (typical hard mask): 800-1500 HV (moderately hard)
- High sp² (graphite-like): 100-300 HV (soft)

Production note: Harder films (more sp³) are more resistant to plasma erosion but more prone to mechanical stress cracking during thermal cycling.

### 2.3.2 Internal Stress and Adhesion

Stress in amorphous carbon films arises from:

1. **Deposition stress** (-500 to -1500 MPa, compressive)
   - Comes from energetic ion bombardment during CVD
   - Higher deposition temperature → lower stress (more relaxation)
   - Higher sp³ content → higher stress (denser packing)

2. **Thermal stress** (-100 to +300 MPa, temperature-dependent)
   - CTE mismatch between carbon and substrate
   - Calculated above: ~±400 MPa during 50°C temperature swing

3. **Total stress during etch:**
   ```
   σ_total = σ_deposition + σ_thermal
   
   During high-temperature plasma (50°C above ambient):
   σ_total = -1000 + (-150) = -1150 MPa (compressive)
   
   This is significant; adhesion of carbon to SiO₂ is typically:
   - Good: >5 GPa (very strong)
   - Marginal: 2-5 GPa (acceptable)
   - Poor: <2 GPa (likely to delaminate)
   ```

**Adhesion failure modes:**

- **Blister/delamination**: Buckling of carbon film at edges or defects
- **Sidewall peeling**: During high-speed sidewall etch, localized tensile stress causes film lifting
- **Interfacial cracking**: Stress concentration at sharp features causes interfacial fracture

Management strategies:
- Use stress-relieved carbon films (lower deposition energy)
- Deposit adhesion layers (Si, TiN) between carbon and SiO₂
- Limit temperature swings during recipe transitions
- Inspect for delamination after every 1000 wafers

---

## 2.4 Oxidation Kinetics & Surface Chemistry

### 2.4.1 Carbon Oxidation in Air and Plasma

Carbon is metastable—it spontaneously oxidizes when exposed to oxygen:

**Air oxidation (slow, room temperature):**
```
2C + O₂ → 2CO   (ΔG = -55 kcal/mol at 25°C, spontaneous)
C + O₂ → CO₂    (ΔG = -94 kcal/mol at 25°C, spontaneous)
```

Kinetics are slow at room temperature (hours to days for visible oxide layer), but oxide layer formation is inevitable.

**Plasma oxidation (fast, high temperature):**

In oxygen-containing plasma (O₂, fluorine + O₂ mixtures), carbon oxidation is much faster:

```
C + F· + O₂ → CF₂O, CFO₂, etc. (oxidized fluorocarbon species)
2C + O₂ → CO₂ (direct oxidation, enhanced by ion bombardment)
```

Rate of oxidation in plasma: ~1-10 nm of oxide per minute (depending on plasma conditions)

### 2.4.2 Surface Oxidation Layer During Etch

During fluorine-based carbon etch in air-exposed chambers:

**Oxidation layer growth:**
```
Time 0-30 seconds: 
  - Initial oxide (10-20 Å) from previous air exposure
  - Oxidation rate: ~0.1 nm/s
  
Time 30-120 seconds:
  - Oxide accumulates to ~100-150 Å
  - Oxidation rate plateaus (oxide acts as diffusion barrier)
  
Steady state:
  - Oxide layer thickness: 100-200 Å (10-20 nm)
  - Etch continues through oxide layer
  - Carbon beneath oxidation layer etches normally
```

**Selectivity consequences:**

Carbon oxide (primarily CO₂ but also CxOy species) etch rate vs. SiO₂:

| Material | Etch Rate (typical) | Selectivity vs. SiO₂ |
|----------|------------------|-------------------|
| Pure carbon (C) | 50-100 nm/min | 5-20:1 |
| Carbon with light oxide (5-10 nm) | 40-80 nm/min | 4-15:1 (slight reduction) |
| Carbon with heavy oxide (>20 nm) | 20-40 nm/min | 2-8:1 (significant reduction) |
| SiO₂ | 5-10 nm/min | 1:1 (reference) |

**Production insight:** Oxidation layer reduces selectivity by 20-50%. Managing oxidation (minimizing or controlled oxidation) is important for maintaining target selectivity.

### 2.4.3 Fluorine-Induced Oxidation Prevention

To maintain high selectivity and minimize oxidation layer:

**Strategy 1: Fluorine atmosphere**
- Use pure F₂ or CF₄ without O₂
- Fluorine radicals compete with oxygen
- F· binds to carbon surface → CF, CF₂ species (prevents O₂ interaction)

**Strategy 2: Radical scavenging**
- Add small amount of reducing agent (H₂ partial pressure)
- H· radicals react with O₂: O₂ + 2H· → 2OH
- Removes free oxygen from plasma

**Strategy 3: Surface temperature control**
- Higher temperature (30-50°C) promotes oxidation
- Lower temperature (-10 to +20°C) slows oxidation
- Production trade-off: Lower T → lower etch rate, better uniformity

---

## 2.5 Electrical Properties & Charging

### 2.5.1 Electrical Conductivity

Conductivity of amorphous carbon varies dramatically with sp²/sp³ ratio:

**Conductivity measurements:**

| Material Type | Conductivity (S/cm) | Resistance (Ω) for 1cm² × 1μm film |
|---------------|-------------------|-------------------------------------|
| Graphite (pure sp²) | 10⁴-10⁵ | 10⁻⁴ Ω (highly conductive) |
| High sp² carbon | 10-10³ | 1-100 Ω |
| Medium (typical hard mask) | 10⁻⁵ to 10⁻³ | 10⁻¹-10² Ω (weakly conductive) |
| High sp³ carbon | 10⁻¹⁵ to 10⁻¹⁰ | >10¹⁰ Ω (insulating) |

**Production implication:**

Weak conductivity (medium sp²/sp³ ratio) creates charging issues during plasma etch:

```
During high-current-density plasma:
  - Electrons strike carbon surface → negative charge accumulation
  - Positive ions (Ar⁺, F⁺) cannot fully neutralize electrons
  - Result: Carbon surface potential reaches -100 to -500V

Consequences of strong charge buildup:
  1. Electrostatic discharge (ESD) damage
  2. Enhanced ion acceleration (ions see higher E-field at surface)
  3. Anomalous ion energy distribution
  4. Pattern distortion from electrostatic forces
```

### 2.5.2 Charge Control Strategies

**Strategy 1: Increase film conductivity**
- Use higher sp² content (graphite-like) carbon
- Trade-off: Reduces etch selectivity, increases roughness

**Strategy 2: Electrical grounding**
- Deposit carbon on conductive substrate (Si or Si with TiN adhesion layer)
- Ensure electrical continuity to ground through substrate
- Most common production approach

**Strategy 3: RF bias electrode grounding**
- Connect RF bias electrode (wafer holder) to ground through matching network
- Forces RF-driven potential swings while maintaining DC path to ground
- Balances charge during etch

**Strategy 4: Chamber pressure tuning**
- Higher pressure → higher ion/neutral collision frequency → better charge neutralization
- Trade-off: Degrades selectivity and reduces etch rate

---

## 2.6 Optical Properties

### 2.6.1 Refractive Index (n) and Extinction Coefficient (k)

Optical properties determine how carbon interacts with diagnostic lasers used for endpoint detection:

**Visible wavelength (λ = 633 nm, red laser common in fabs):**

| Carbon Type | Refractive Index (n) | Extinction Coefficient (k) | Appearance |
|-------------|-------------------|--------------------------|-----------|
| Graphite (pure) | 2.5-3.0 | 1.5-2.0 | Black, opaque |
| High sp² (soft) | 2.0-2.5 | 1.0-1.5 | Dark gray |
| Medium (typical hard mask) | 1.5-2.0 | 0.5-1.0 | Dark |
| High sp³ (hard) | 1.2-1.5 | 0.1-0.3 | Tan/transparent |

**Reflectance at 633 nm:**
```
R = [(n-1)² + k²] / [(n+1)² + k²] × 100%

For typical hard mask carbon (n=1.7, k=0.8):
R ≈ 40-50% reflectance

This is important for optical endpoint detection:
- Higher R → brighter signal (easier to detect)
- Lower R → dimmer signal (harder to detect)
- Graphite-like carbon (~50% R) is better for optical monitoring
- Diamond-like carbon (~20% R) is worse for optical monitoring
```

### 2.6.2 Endpoint Detection Implications

Optical monitoring of carbon etch uses laser reflectance:

```
During etch:
  Time 0-T_etch: Carbon film in place (high reflectance, ~45%)
  Time T_etch: Carbon fully removed (substrate now exposed, ~0% if SiO₂)
  
Endpoint = when reflectance drops sharply

Challenge: If carbon has low reflectance and substrate has intermediate reflectance,
the signal change is small, making endpoint detection difficult.

Solution: Use electrical monitoring (RF impedance change) as supplement to optical.
```

---

## 2.7 Comparison of Carbon Variants

Different carbon deposition methods produce films with different properties:

### 2.7.1 Carbon Types in Production

**Soft Carbon (PECVD, low energy):**
- sp²/sp³ ratio: ~70:30 (mostly sp², graphite-like)
- Thermal conductivity: 20-30 W/m·K (low)
- Hardness: 300-500 HV (soft)
- Etch rate: 80-150 nm/min (fast)
- Selectivity vs. SiO₂: 3-8:1 (moderate)
- Electrical conductivity: 10-100 S/cm (conductive)
- **Application**: General-purpose hard mask, fast etch, less stringent selectivity

**Hard Carbon (PECVD, high energy):**
- sp²/sp³ ratio: ~30:70 (mostly sp³, diamond-like)
- Thermal conductivity: 80-150 W/m·K (better)
- Hardness: 1500-3000 HV (hard)
- Etch rate: 30-80 nm/min (slow)
- Selectivity vs. SiO₂: 10-50:1 (excellent)
- Electrical conductivity: 10⁻⁶-10⁻³ S/cm (insulating)
- **Application**: Critical applications requiring selectivity, temperature-stable

**Hydrogenated Carbon (a-C:H):**
- Composition: Carbon + 10-30% hydrogen
- Hardness: 800-2000 HV (medium)
- Etch rate: 50-100 nm/min
- Selectivity vs. SiO₂: 5-15:1 (very good)
- Thermal stability: Lower (hydrogen release at elevated T)
- **Application**: Cost-effective alternative, good selectivity, limited thermal tolerance

**Nitrogenated Carbon (N-doped):**
- Composition: Carbon + 1-5% nitrogen
- Hardness: 1000-2500 HV
- Etch rate: 40-90 nm/min
- Selectivity vs. SiO₂: 15-30:1 (excellent, N improves selectivity)
- Oxidation resistance: Better (N prevents oxidation)
- **Application**: Advanced nodes, excellent selectivity and oxidation resistance

### 2.7.2 Production Selection Criteria

Fab selection of carbon type depends on:

| Criterion | Prioritizes | Best Choice |
|-----------|------------|-------------|
| High etch rate needed | Speed | Soft Carbon |
| Selectivity critical | Selectivity | Hard Carbon or N-doped |
| Temperature stability | Process control | Hard Carbon |
| Cost minimization | Economics | Soft Carbon or a-C:H |
| Oxidation resistance | Long process windows | N-doped Carbon |
| CTE matching to substrate | Thermal stability | Si₃N₄-embedded Carbon |

---

## 2.8 Integration Considerations

### 2.8.1 Carbon on Different Substrates

Carbon hard mask in real 3D NAND uses different underlying layers:

**Carbon on SiO₂:**
- Adhesion: Good (Si-O bonding with carbide interface)
- CTE mismatch: High (~4.5 ppm/°C difference)
- Thermal stress: Compressive during etch (wafer heats up)
- Oxidation layer formation: Moderate (SiO₂ substrate has O₂)
- Selectivity: Good (C/SiO₂ ~10-20:1)

**Carbon on Si₃N₄:**
- Adhesion: Excellent (N-C bonding strong)
- CTE mismatch: Low (~2 ppm/°C difference)
- Thermal stress: Minimal (good CTE match)
- Oxidation layer formation: Low (N resists oxidation)
- Selectivity: Excellent (C/Si₃N₄ very selective)

**Carbon on Si:**
- Adhesion: Moderate (Si-C bonding weaker than Si-O)
- CTE mismatch: Very low (~2 ppm/°C difference)
- Thermal stress: Minimal
- Oxidation layer formation: High (Si oxidizes quickly)
- Selectivity: Good (C/Si ~5-15:1)

**Production practice:**
Most fabs use adhesion layers:
```
Substrate (SiO₂) → Adhesion layer (TiN, 5-10 nm) → Hard mask carbon (20-50 nm)
```

Adhesion layer benefits:
- Prevents direct SiO₂-carbon interface (reduces interfacial stress)
- Improves electrical contact (TiN is conductive)
- Blocks oxygen diffusion (TiN acts as barrier)

---

## 2.9 Surface Morphology Evolution During Etch

### 2.9.1 Initial Surface Roughness

As-deposited carbon films have initial surface roughness:

**Roughness Amplitude (RMS):**
- Soft carbon (PECVD): Ra = 1-3 nm (relatively smooth)
- Hard carbon (PECVD): Ra = 0.5-2 nm (very smooth, low defect density)
- Hydrogenated carbon: Ra = 2-5 nm

This initial roughness is important for subsequent lithography and selectivity.

### 2.9.2 Roughness Evolution During Etch

As carbon etch proceeds, surface roughness can increase or decrease depending on process conditions:

**Low ion energy (<50 eV) etch:**
- Smooth chemical etching dominates
- Roughness increases slowly (Ra → Ra + 1-2 nm)
- Selectivity excellent
- Morphology: Smooth, wave-like pattern

**Medium ion energy (50-100 eV) etch:**
- Balanced chemical + ion-assisted etching
- Roughness increases moderately (Ra → Ra + 2-4 nm)
- Selectivity good
- Morphology: Faceted, ion-induced texture

**High ion energy (>100 eV) etch:**
- Ion sputtering dominates
- Roughness increases rapidly (Ra → Ra + 5-10 nm)
- Roughness becomes noisy, non-uniform
- Selectivity degrades (high ion energy sputters all materials)
- Morphology: Rough, spiky, crystalline-like

**Production consequence:** Process recipes optimize for medium ion energy to balance selectivity and surface quality.

---

## 2.10 Summary & Key Takeaways

1. **sp²/sp³ Ratio is Critical**: Higher sp² (graphite-like) → faster etch but more conductive; Higher sp³ (diamond-like) → slower etch but better selectivity

2. **Thermal Conductivity Drives Design**: Carbon's low thermal conductivity (20-100 W/m·K) requires active wafer temperature control and limits passive cooling

3. **Thermal Expansion Mismatch**: CTE difference (~4.5 ppm/°C) between carbon and SiO₂ creates compressive stress during etch, risking delamination

4. **Oxidation Layer Impacts Selectivity**: Oxide layer (10-200 Å) reduces selectivity by 20-50%; controlled oxidation or oxidation prevention essential

5. **Charge Buildup is Real**: Weak conductivity causes electrostatic accumulation (-100 to -500V); conductive substrate grounding and charge control essential

6. **Optical Properties Affect Monitoring**: Carbon's refractive index (1.5-2.0) affects endpoint detection; graphite-like carbon better for optical monitoring

7. **Material Variants Available**: Soft/hard/hydrogenated/nitrogenated carbons trade off etch rate, selectivity, thermal stability, cost

8. **Substrate Interaction Matters**: Adhesion layers (TiN) improve stability and electrical properties; CTE matching (Si₃N₄ substrate) minimizes thermal stress

9. **Surface Evolution Matters**: Initial roughness increases during etch by 2-10 nm depending on ion energy; medium ion energy balances selectivity and smoothness

---

## Study Questions

1. Explain why amorphous carbon films contain both sp² and sp³ bonded carbon, and describe how this mixed bonding affects plasma etch behavior.

2. Calculate the temperature rise on a 300mm wafer receiving 1000 W total power dissipation, assuming carbon thermal conductivity of 50 W/m·K and a thermal resistance of 0.05 K·cm²/W between wafer and electrode.

3. Describe the CTE mismatch between carbon and SiO₂, and explain how this creates mechanical stress during a temperature swing of ±50°C.

4. Why does oxidation of carbon during etch reduce selectivity vs. SiO₂, and what strategies minimize carbon oxidation?

5. Explain charge buildup on weakly conductive carbon films during plasma etch, and describe three methods to control electrostatic potential.

6. Compare optical properties of soft carbon (graphite-like) vs. hard carbon (diamond-like), and discuss implications for endpoint detection.

---

## References & Further Reading

- Narasimhan, S., et al., "Optical and Structural Properties of Amorphous Carbon Films," IEEE Transactions on Plasma Science, Vol. 35, 2007
- Gilkes, K.W., et al., "Direct Detection of sp³ Bonding in Diamond-like Carbon Films by NMR," Physical Review B, Vol. 51, 1995
- Freund, L.B., Suresh, S., "Thin Film Materials: Stress, Defect Formation, and Surface Evolution," Cambridge University Press, 2003
- Mao, D., et al., "Plasma Etching of Amorphous Carbon in Fluorine-Based Plasmas," Journal of Vacuum Science & Technology, Vol. 28, 2010

**Next Chapter:** [Chapter 3: Fluorine Chemistry in Carbon Plasma Etch](./03-fluorine-chemistry.md)

---

**Chapter 2 Development Status:** Comprehensive content complete  
**Last Updated:** October 3, 2026  
**Version:** 1.0 (Complete technical chapter)
