# Chapter 3: Fluorine Chemistry in Carbon Plasma Etch (F₂, CF₄, C₂F₆, CHF₃)

## Overview

Fluorine is the primary reactive species in carbon hard mask etch, responsible for the chemical removal of carbon and selectivity against underlying materials. Understanding the chemistry of fluorine—how it is generated from molecular precursors, how it attacks carbon, and how it forms product species—is essential for designing efficient etch processes and solving production challenges.

**Learning Objectives:**
- Understand fluorine radical generation mechanisms in ICP/CCP systems
- Quantify fluorine reactivity with carbon vs. other materials
- Analyze gas chemistry options (F₂, CF₄, C₂F₆, CHF₃) and their trade-offs
- Recognize fluorocarbon polymer formation and control strategies
- Design gas mixtures for selectivity and etch rate targets

---

## 3.1 Fluorine Radical Generation

### 3.1.1 Dissociation Pathways

In plasma, fluorine-containing molecules are dissociated into reactive atoms through electron impact:

**Primary dissociation mechanisms:**

```
Electron-Impact Dissociation:

1. F₂ dissociation:
   e⁻ + F₂ → e⁻ + F· + F·        (direct dissociation)
   or
   e⁻ + F₂ → F₂⁻ + hν            (temporary negative ion)
   F₂⁻ → F· + F⁻ (fast decay)
   
   Ionization threshold: 2.9 eV (low threshold → efficient dissociation)

2. CF₄ dissociation:
   e⁻ + CF₄ → e⁻ + CF₃· + F·     (single F extraction)
   or
   e⁻ + CF₄ → e⁻ + CF₂· + F₂     (double F, different fragments)
   or
   e⁻ + CF₄ → CF₃⁻ + F           (negative ion pathway)
   
   Ionization threshold: 13.0 eV (higher threshold → less efficient)

3. C₂F₆ dissociation:
   e⁻ + C₂F₆ → e⁻ + CF₃· + CF₃· (symmetric cleavage)
   or
   e⁻ + C₂F₆ → e⁻ + CF₂· + CF₃F (asymmetric cleavage)
   
   Ionization threshold: 11.5 eV (high threshold)

4. CHF₃ dissociation:
   e⁻ + CHF₃ → e⁻ + CHF₂· + F·   (one F extraction)
   or
   e⁻ + CHF₃ → e⁻ + CF₂· + H·    (H + CF₂ formation)
   
   Ionization threshold: 13.5 eV (high)
```

### 3.1.2 Electron Temperature Dependence

The efficiency of dissociation depends on electron temperature (T_e):

**Typical electron temperatures in ICP/CCP systems:**
- ICP (high power): T_e = 3-5 eV
- CCP (moderate power): T_e = 2-4 eV
- Low-power CCP: T_e = 1-2 eV

**Dissociation efficiency vs. T_e:**

| Gas | Threshold (eV) | T_e = 2 eV | T_e = 3 eV | T_e = 4 eV | T_e = 5 eV |
|-----|---------------|-----------|-----------|-----------|-----------|
| F₂ | 2.9 | ~40% | ~60% | ~75% | ~85% |
| CF₄ | 13.0 | ~5% | ~15% | ~30% | ~50% |
| C₂F₆ | 11.5 | ~3% | ~10% | ~25% | ~45% |
| CHF₃ | 13.5 | ~2% | ~8% | ~20% | ~40% |

**Production insight:** Higher T_e (higher RF power) produces more atomic fluorine from high-threshold molecules. F₂, with its low threshold, is very efficient across all operating conditions.

### 3.1.3 Ion Formation

Beyond atomic radicals, ions are also generated:

**Ion formation pathways:**

```
Ionization (creating charged species):

1. Direct ionization:
   e⁻ + F₂ → 2e⁻ + F₂⁺           (removes one electron)
   
2. Dissociative ionization:
   e⁻ + CF₄ → 2e⁻ + CF₃⁺ + F·    (breaks molecule, creates cation)

3. Charge exchange:
   F· + Ar⁺ → F⁺ + Ar            (fast ion-molecule reaction)
   
4. Ion-molecule reactions (in plasma):
   F₂⁺ + e⁻ → F· + F· (fast recombination)
   CF₄⁺ + CF₄ → CF₅⁺ + CF₃ (ion growth)
```

**Ion species in fluorine/carbon plasma:**
- F⁺ (atomic fluorine ion)
- CF⁺ (fluorocarbon ion)
- CF₂⁺, CF₃⁺, CF₄⁺, CF₅⁺ (higher fluorocarbon ions)
- Ar⁺ (from argon diluent)

These ions are critical for ion-assisted etching (discussed in Chapter 4).

---

## 3.2 Fluorine Reactivity with Carbon

### 3.2.1 Primary Etch Reaction

The fundamental carbon etch reaction is:

```
C + 2F· → CF₂ + energy release

Thermodynamics:
ΔH_rxn = -515 kJ/mol (highly exothermic, spontaneous)
ΔG_rxn = -510 kJ/mol (highly favorable)

This reaction is highly favorable, meaning fluorine radicals very efficiently
attack and etch carbon. It's one of the fastest known etch reactions.
```

**Reaction mechanism (step-by-step):**

```
Step 1: F· approach to carbon surface
        Collision cross-section: ~1-5 × 10⁻¹⁵ cm² (large)
        High probability of collision per surface F·

Step 2: F· radical attacks C-C bond
        C-C bond (sp³): 348 kJ/mol binding energy
        F· is highly reactive (unpaired electron)
        Forms transient C-F species

Step 3: Second F· attacks same carbon
        Partially fluorinated C becomes more reactive
        C-F bond (348 kJ/mol) + C-F (348 kJ/mol) → CF₂

Step 4: CF₂ desorption
        CF₂ is volatile at typical etch temperatures
        Vapor pressure of CF₂: 8 MPa at 0°C (very high)
        CF₂ desorbs rapidly → leaves carbon surface etched

Net result: One carbon atom removed, two fluorine radicals consumed
           Very efficient etch
```

### 3.2.2 Etch Rate Kinetics

The etch rate depends on fluorine radical flux and carbon reactivity:

**Etch rate equation:**

```
R_etch = k_chem × [F·] × σ_rxn × exp(-E_a / kT)

where:
  k_chem = chemical rate constant
  [F·] = fluorine radical concentration (measured in flux at surface)
  σ_rxn = reaction cross-section (~2 × 10⁻¹⁵ cm²)
  E_a = activation energy for reaction (~10-15 kcal/mol)
  T = surface temperature
```

**Typical etch rates:**

| Gas Chemistry | Pressure | Power | T_surf | R_etch |
|---------------|----------|-------|--------|--------|
| F₂ 30 sccm | 50 mTorr | 2000 W | 20°C | 150-200 nm/min |
| CF₄ 50 sccm | 75 mTorr | 2000 W | 20°C | 50-80 nm/min |
| C₂F₆ 50 sccm | 75 mTorr | 2000 W | 20°C | 30-60 nm/min |
| CHF₃ 50 sccm | 75 mTorr | 2000 W | 20°C | 40-70 nm/min |

**Key observations:**
- F₂ is 2-5× faster than CF₄ (higher F· generation efficiency)
- CF₄, CHF₃, C₂F₆ have similar etch rates (different dissociation pathways converge)
- Etch rate strongly depends on [F·], which depends on gas flow, plasma power, pressure

### 3.2.3 Etch Rate vs. Temperature

Temperature significantly affects etch rate via Arrhenius dependence:

```
Etch rate temperature sensitivity:

For E_a = 12 kcal/mol (typical for carbon/fluorine):

R_etch(T) / R_etch(T₀) = exp[-E_a/R × (1/T - 1/T₀)]

At T = 20°C (293 K): R_etch = R₀ (reference)
At T = 30°C (303 K): R_etch = R₀ × 1.13 (13% increase)
At T = 40°C (313 K): R_etch = R₀ × 1.28 (28% increase)
At T = 50°C (323 K): R_etch = R₀ × 1.46 (46% increase)

Sensitivity: ~10-13% per 10°C
```

This high temperature sensitivity is why wafer temperature control (±2°C) is critical for etch uniformity.

---

## 3.3 Fluorocarbon Polymer Formation

### 3.3.1 Polymer Growth Mechanisms

During carbon etch, fluorine-rich polymer layers accumulate on surfaces:

**Polymer deposition pathways:**

```
1. Direct radical polymerization:
   F· + C → CF
   CF + F· → CF₂
   CF₂ + CF₂ → (CF₂)₂ (polymer growth)
   (CF₂)ₙ + F· → (CF₂)ₙ₊₁ (continuation)

2. Ion-driven polymer formation:
   F⁺, CF⁺ strike surface → create active sites
   Radicals rapidly polymerize at active sites
   Higher ion energy → faster polymer nucleation

3. Radical coupling:
   CF₃ + CF₂ → CF₃-CF₂ (C-C bond formation in polymer)
   (CF₃-CF₂)ₙ forms backbone
```

**Polymer composition:**

Typical polymer formula: (CₓFᵧ)ₙ

Common polymer compositions:
- CF₀.₈ (fluorine-rich, typical): Most common, highest reactivity
- CF₁.₀ (stoichiometric CF)
- CF₁.₅ (CF₂/CF mixture, intermediate)
- CF₂.₀ (pure CF₂, rare, very stable)

**Polymer layer growth rate:**
```
Deposition rate: 1-10 nm/minute (depends heavily on conditions)

At low ion energy (chemical etch dominant):
  Deposition rate ~ 2-5 nm/minute
  
At high ion energy (ion-assisted etch):
  Deposition rate ~ 5-15 nm/minute (higher energy drives more polymerization)
```

### 3.3.2 Polymer Role During Etch: Passivation

The fluorocarbon polymer layer plays a critical **positive role** during etch:

```
Polymer Passivation Mechanism:

Carbon surface structure:
  ├─ Bare carbon (high reactivity)
  │  Etch rate: Fast
  │  Selectivity: Lower (aggressive etch)
  │  Sidewalls: Rough (non-directional)
  │
  └─ Polymer-passivated carbon (lower reactivity)
     Etch rate: Slower (polymer blocks radicals)
     Selectivity: Higher (only ion-bombarded surfaces etch)
     Sidewalls: Smooth, vertical (ion-directional etch)
```

**How polymer passivation improves selectivity:**

On carbon surface with polymer coating:
- Vertical surfaces (perpendicular to ion beam) are ion-bombarded
- Ions remove polymer layer → expose carbon → etch
- Horizontal surfaces (parallel to ion beam) are NOT ion-bombarded
- Polymer accumulates → blocks fluorine → no etch

Result: **Directional (anisotropic) etch** with excellent selectivity to horizontal surfaces.

**Selectivity improvement quantified:**

| Condition | C/SiO₂ Selectivity | Sidewall Angle |
|-----------|------------------|-----------------|
| No polymer (bare carbon) | 5-8:1 | Sloped (isotropic) |
| Light polymer (5-10 nm) | 10-15:1 | Nearly vertical |
| Moderate polymer (10-20 nm) | 20-30:1 | Vertical |
| Heavy polymer (>30 nm) | 30-50:1 | Vertical + recess |

Production consequence: Polymer management is not "remove residue at end of etch"; it's integral to achieving required selectivity and anisotropy during etch.

### 3.3.3 Polymer Removal: The Challenge

After etch, accumulated polymer must be removed before subsequent processes:

**Why polymer removal is critical:**

1. **ALD/CVD blocking**: Polymer layer prevents conformal deposition
2. **Electrical performance**: Polymer residue affects interconnect resistance
3. **Yield loss**: Unpredictable polymer creates process variation

**Polymer removal methods:**

| Method | Temperature | Time | Pros | Cons |
|--------|-------------|------|------|------|
| O₂ plasma ashing | 80-120°C | 5-20 min | Effective, in-situ | Can damage carbon |
| Thermal annealing | 100-150°C | 30-60 min | Non-destructive | Slow, requires separate tool |
| Solvent (wet etch) | Room temp | 30-60 min | Effective | Ex-situ, handling risk |
| HNO₃ wet clean | Room temp | 15-30 min | Effective, selective | Corrosive, disposal issues |
| Direct fluorine strip | Low temp | 5-10 min | Fast, in-situ | Aggressive, residue damage risk |

**Polymer decomposition kinetics:**

```
O₂ plasma ashing:
(CₓFᵧ)ₙ + O₂ → CO₂ + CO + COF₂ + other products (volatile)

Thermal decomposition:
(CₓFᵧ)ₙ →^T CF₂ + CF + CO (gaseous products at 100-150°C)

Wet HNO₃:
(CₓFᵧ)ₙ + HNO₃ → dissolved species + CO₂ + CF products
```

**Production challenge**: Some polymer species are volatile at 80-100°C (F/C rich), others resist decomposition until 150°C+ (C-rich polymers). Optimizing removal requires controlling polymer composition during etch, not just trying to strip it afterwards.

---

## 3.4 Gas Chemistry Selection

### 3.4.1 Pure Fluorine (F₂)

**Properties:**
- Molecular formula: F₂
- Vapor pressure: 5 MPa at 0°C (highly volatile, must use gas from bottle)
- Dissociation threshold: 2.9 eV (very efficient)
- Fluorine atom yield: ~90% from electron dissociation

**Advantages:**
- Highest etch rate (150-200 nm/min typical)
- Simplest chemistry (pure F· → CF₂)
- Excellent selectivity (no carbon in feedstock)

**Disadvantages:**
- Most aggressive gas (rapid etch, difficult to control)
- Corrosive to chamber walls (stainless steel attacked by F₂)
- Expensive (pure F₂ must be generated or purchased)
- Hazardous (F₂ is toxic gas)

**Production use:**
- Limited to specialized processes requiring maximum etch rate
- Most modern fabs avoid F₂ due to handling and safety costs
- Restricted to facilities with advanced gas safety infrastructure

---

### 3.4.2 Carbon Tetrafluoride (CF₄)

**Properties:**
- Molecular formula: CF₄
- Molar mass: 88 g/mol
- Dissociation threshold: 13.0 eV
- Fluorine atom yield: ~60-70% from electron dissociation

**Advantages:**
- Balanced etch rate (50-80 nm/min)
- Moderate gas toxicity (safer than F₂)
- Self-limiting chemistry (CF₄ contains carbon, less selective but more uniform)
- Widely available, lowest cost
- Stable, easy to handle

**Disadvantages:**
- Moderate selectivity (CF₄ provides some carbon feedback)
- Requires moderate electron temperature for efficient dissociation
- Polymer deposition moderate

**Production use:**
- Industry standard for most carbon etch processes
- ~70% of production fabs use CF₄ as primary gas
- Excellent cost-to-performance trade-off

---

### 3.4.3 Hexafluoroethane (C₂F₆)

**Properties:**
- Molecular formula: C₂F₆
- Molar mass: 138 g/mol
- Dissociation threshold: 11.5 eV
- Fluorine atom yield: ~50-60%

**Advantages:**
- Lower etch rate (30-60 nm/min) → easier control
- Excellent selectivity (two carbons per molecule reduce F· availability)
- Better uniformity (self-limiting chemistry)
- Lower polymer deposition (larger molecule → more CF₂ formation)
- Safer, less aggressive gas

**Disadvantages:**
- Slower etch (requires longer time or higher power for same etch depth)
- More expensive than CF₄
- Lower dissociation efficiency (higher threshold)

**Production use:**
- Used when selectivity is critical (metal/dielectric stacks)
- Becoming more popular as features shrink (better control)
- ~20% of production fabs use C₂F₆

---

### 3.4.4 Trifluoromethane (CHF₃)

**Properties:**
- Molecular formula: CHF₃
- Molar mass: 70 g/mol
- Dissociation threshold: 13.5 eV
- Hydrogen content: ~1.4%

**Advantages:**
- Hydrogen modifies selectivity (H· can react with oxygen, reduces oxidation)
- Moderate etch rate (40-70 nm/min)
- Better selectivity than CF₄
- Lower polymer deposition (H helps decompose polymer)
- Useful for oxygen-containing chemistries

**Disadvantages:**
- More complex chemistry (H adds variables)
- Higher threshold → needs higher T_e for good dissociation
- Hydrogen can create reducing species (may affect other materials)

**Production use:**
- Specialized applications requiring hydrogen benefits
- ~10% of production fabs use CHF₃
- Growing interest for advanced selectivity control

---

## 3.5 Binary & Ternary Gas Mixtures

### 3.5.1 Carbon + Inert Mixtures

**CF₄ + Ar (most common production recipe):**

```
Typical ratio: 20% CF₄ / 80% Ar (by partial pressure)

Effects of argon diluent:
- Argon provides ion source (Ar⁺ for ion bombardment)
- Argon increases collision frequency (higher pressure operability)
- Argon is inert (does not alter etch chemistry)

Etch rate vs. Ar fraction:
  Pure CF₄: 80 nm/min (very fast, hard to control)
  CF₄ + 50% Ar: 65 nm/min
  CF₄ + 80% Ar: 50 nm/min (moderate, typical production)
  CF₄ + 95% Ar: 35 nm/min (very slow, excellent control)
```

**C₂F₆ + Ar (conservative, high-selectivity recipes):**

```
Typical ratio: 15% C₂F₆ / 85% Ar

Provides slow, controlled etch with excellent selectivity:
  Etch rate: 30-50 nm/min
  Selectivity C/SiO₂: 20-30:1
  Etch uniformity: Excellent
```

### 3.5.2 Carbon + Oxygen Mixtures

**CF₄ + O₂ (oxidation-assisted etch):**

```
Typical ratio: 10-20% O₂ addition to CF₄/Ar base recipe

Mechanism of O₂ effect:
- O₂ is dissociated into O· atoms
- O· + C → CO, CO₂ (oxidative etch pathway)
- Combined fluorine + oxygen etch is faster than fluorine alone
- Reduces polymer deposition (O· decomposes polymers)

Effects on process:
  Etch rate increase: +20-40%
  Selectivity change: Slightly reduced
  Polymer deposition: Significantly reduced
  Surface morphology: Smoother (less polymer residue)
```

**Trade-off: O₂ increases etch rate but at cost of selectivity and oxidation layer formation on carbon.**

### 3.5.3 Carbon + Hydrogen Mixtures

**CHF₃ + Ar (hydrogen addition):**

```
Mechanism of H· effect:
- Hydrogen radicals generated from CHF₃ dissociation
- H· reacts with oxygen: O₂ + 2H· → 2OH (prevents oxidation)
- H· affects polymer: (CₓFᵧ) + H· → CₓFᵧ₋₁ + HF (polymer reduction)

Benefits:
- Reduced oxidation (H scavenges free oxygen)
- Reduced polymer accumulation
- Better selectivity through oxidation prevention

Drawback:
- More complex chemistry
- Hydrogen can create HF (corrosive to SiO₂)
```

### 3.5.4 Recipe Optimization Strategy

Production fabs optimize gas mixtures by:

```
1. Base selection (F-source):
   → Choose CF₄ for balanced recipes
   → Choose F₂ if max speed needed
   → Choose C₂F₆ if selectivity critical

2. Diluent selection (typically Ar):
   → Adjust etch rate via Ar %
   → Higher Ar → slower, more uniform
   → Lower Ar (more F) → faster, less controlled

3. Additive optimization:
   → Add O₂ if speed boost needed
   → Add H₂ (via CHF₃) if oxidation/polymer problematic
   → Keep additives <20% (major additives complicate plasma)

4. Empirical tuning:
   → Vary gas ratios on test wafers
   → Measure etch rate, selectivity, uniformity
   → Optimize for specific target specs
```

---

## 3.6 Selectivity Mechanisms: Chemistry Basis

### 3.6.1 Differential Etch Rate Chemistry

Different materials have different reaction rates with fluorine:

**Etch rate hierarchy (fluorine chemistry dominated):**

```
Ranking by etch rate (fast to slow):
1. Carbon (pure): ~100 nm/min (very fast)
2. SiC: ~80 nm/min (fast, contains carbon)
3. Silicon: ~30 nm/min (moderate, Si-F bonds form)
4. SiO₂: ~5-10 nm/min (slow, SiF₄ is stable)
5. Si₃N₄: ~3-8 nm/min (slow, SiF₄ or nitride species)
6. Metal oxides (Al₂O₃, TiO₂): ~2-5 nm/min (very slow)
7. Metals (Al, Ti): ~1-2 nm/min via oxidation (extremely slow)
```

**Selectivity ratios (experimental):**

| Target | Mask Material | Over-layer | Selectivity |
|--------|--------------|-----------|-------------|
| SiO₂ | Carbon | - | 10-20:1 |
| Si | Carbon | SiO₂ | 5-15:1 |
| Si₃N₄ | Carbon | - | 15-30:1 |
| Metal (via hole) | Carbon | SiO₂ | 5-10:1 |

### 3.6.2 Ion Energy Selectivity Enhancement

While chemical selectivity sets a baseline, ion energy further modifies selectivity:

**At high ion energy (>100 eV):**
- Sputtering dominates over chemistry
- Selectivity degrades (hard to distinguish materials)
- All materials sputter at similar rates

**At medium ion energy (50-100 eV):**
- Ion-assisted chemistry dominates
- Selectivity preserved
- Different materials respond differently to ion bombardment

**At low ion energy (<30 eV):**
- Chemical etch dominates
- Highest selectivity
- But reduced etch rate

**Practical trade-off**: Production recipes use medium ion energy (60-80 eV typical) to balance selectivity and etch rate.

---

## 3.7 Endpoint Detection via Chemistry

### 3.7.1 Optical Emission Spectroscopy (OES)

Gas-phase products from etch can be detected optically:

**Key species in carbon/fluorine etch plasma:**

| Species | Wavelength | Intensity | Note |
|---------|-----------|-----------|------|
| F atom | 703.7 nm | Strong | Direct measure of F· |
| CF radical | 290-320 nm | Moderate | Product species |
| CO | 200-300 nm | Variable | Oxidation product |
| H_α | 656 nm | Weak (if H₂ present) | Hydrogen indicator |
| Ar | 750-850 nm | Strong | Inert baseline |

**Endpoint detection principle:**
- Monitor CF or CO emission during etch
- When carbon is consumed, CF signal drops (etch complete)
- Endpoint = when CF/Ar ratio falls below threshold

**Limitations:**
- Requires direct optical path to plasma
- Signal can be noisy (interference from other species)
- Substrate effects (exposed SiO₂ creates different spectrum)

### 3.7.2 Electrical Monitoring (RF Impedance)

RF impedance of the plasma changes as etch progresses:

```
Impedance evolution during carbon etch:

Time 0 (start):
  Carbon film in place: Good electrical contact
  Impedance: Z₁ (~100-150 Ω)

Time T_etch/2:
  Carbon partially etched: Impedance increasing
  Impedance: Z₂ (~150-200 Ω)
  
Time T_etch (complete):
  Carbon removed, substrate (SiO₂) exposed
  SiO₂ is insulating: High impedance
  Impedance: Z₃ (~250-400 Ω)
```

**Electrical endpoint detection:**
- Monitor RF voltage and current during etch
- Calculate impedance (V/I)
- When impedance crosses threshold → endpoint

**Advantages over optical:**
- No optical path needed (works in opaque chambers)
- More robust (electrical signal is direct)
- Better for carbon detection (big impedance change)

---

## 3.8 Production Gas Economics

### 3.8.1 Cost Comparison

**Gas costs for carbon etch ($/wafer, 300mm platform):**

| Gas | Cost/wafer | Volume used | Total Gas Cost |
|-----|-----------|------------|-----------------|
| F₂ | $8-12 | 20-40 sccm | $8-12/wafer |
| CF₄ | $2-4 | 40-80 sccm | $2-4/wafer |
| C₂F₆ | $4-7 | 30-60 sccm | $4-7/wafer |
| CHF₃ | $5-8 | 30-60 sccm | $5-8/wafer |

**Production choice rationale:**

Most fabs use CF₄ because:
- Lowest cost ($2-4/wafer)
- Adequate selectivity for most applications
- Wide availability and stable supply
- Mature process knowledge across industry

Advanced fabs use C₂F₆ when:
- Selectivity is critical (advanced nodes)
- Process margin important (risk reduction)
- Cost difference acceptable for yield improvement

---

## 3.9 Safety & Environmental Considerations

### 3.9.1 Fluorine Gas Hazards

**Exposure limits:**

| Gas | PEL (8hr TWA) | IDLH | Hazard |
|-----|--------------|------|--------|
| F₂ | 1 ppm | 25 ppm | Highly toxic, corrosive |
| CF₄ | 5000 ppm | > 50,000 ppm | Asphyxiant, greenhouse gas |
| C₂F₆ | 5000 ppm | > 50,000 ppm | Asphyxiant, greenhouse gas |
| CHF₃ | 5000 ppm | > 50,000 ppm | Asphyxiant, greenhouse gas |

F₂ requires special handling; CF₄/C₂F₆/CHF₃ are relatively safe but require ventilation.

### 3.9.2 Wastewater Treatment

Fluorocarbon etch produces fluoride-containing waste:

**Removal methods:**
- Calcium hydroxide addition: CaF₂ precipitation
- Ion exchange: Fluoride capture on resin
- Thermal decomposition: High-temperature treatment

**Cost**: $0.50-1.50/wafer for proper fluoride removal

---

## 3.10 Summary & Key Takeaways

1. **Fluorine Generation Efficiency Varies**: F₂ most efficient (90% dissociation), CF₄/C₂F₆/CHF₃ less efficient (50-70%); choose based on speed vs. control

2. **Etch Chemistry is Exothermic**: C + 2F· → CF₂ is highly favorable (ΔG = -510 kJ/mol), making carbon etching very fast and efficient

3. **Fluorocarbon Polymers are Essential**: Polymer passivation provides directional, selective etch; removal is post-etch integration challenge, not sign of "bad" etch

4. **Gas Mixture Selection is Critical**: CF₄ is production standard; C₂F₆ for selectivity; F₂ for speed; additives (O₂, H₂) modify selectivity and polymer

5. **Temperature Sensitivity is High**: ~10%/10°C etch rate change; requires ±2°C wafer temperature control for etch uniformity

6. **Selectivity Mechanisms are Dual**: Chemical selectivity (material-dependent etch rates) + ion energy selectivity (differential sputtering)

7. **Endpoint Detection is Challenging**: Optical (OES) and electrical (RF impedance) both have merits; best fabs use redundant monitoring

---

## Study Questions

1. Calculate the dissociation fraction of CF₄ in a CCP plasma with electron temperature of 3 eV, using the table in Section 3.1.2.

2. Explain why pure F₂ etch is rarely used in production despite having the highest etch rate.

3. Describe the polymer passivation mechanism and explain how it improves directional selectivity during carbon etch.

4. Compare CF₄, C₂F₆, and CHF₃ as primary fluorine sources. Which would you choose for (a) maximum etch rate, (b) best selectivity, (c) lowest cost?

5. Explain how oxygen additives (O₂) affect etch rate, selectivity, and polymer formation in carbon etch.

6. Design a gas mixture recipe (base gas + diluent + optional additives) for etching a 30 nm carbon hard mask with target selectivity >15:1 vs. SiO₂. Justify your choices.

---

## References & Further Reading

- Lieberman, M.A., Lichtenberg, A.J., "Principles of Plasma Discharges and Materials Processing," 2nd ed., Wiley, 2005
- Colombo, V., et al., "Fluorine Chemistry in Plasma Etching," IEEE Transactions on Plasma Science, Vol. 27, 1999
- Coburn, J.W., "Plasma-Assisted Etching," Applied Physics Letters, Vol. 42, 1983

**Next Chapter:** [Chapter 4: Plasma-Carbon Surface Reactions & Ion-Assisted Etching](./04-plasma-carbon-reactions.md)

---

**Chapter 3 Development Status:** Comprehensive content complete  
**Last Updated:** October 3, 2026  
**Version:** 1.0 (Complete technical chapter)
