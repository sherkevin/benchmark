# Notes for Tc Prediction Task

## Task Overview
I need to predict superconducting critical temperature (Tc) for 11 target compositions of Gd-based RE(1-x)Pr(x)Ba2Cu3O7. I have synthesized samples with Er and Nd as RE elements to measure, which should help me understand how Tc depends on Pr content (x) and RE element.

## Plan
1. Measure Tc for synthesized Er and Nd samples to establish relationship
2. Analyze pattern to infer law governing Tc vs x and RE
3. Apply law to predict Tc for Gd targets
4. Determine best composition and critical Pr content

## Available Samples
From synthesized.txt:
- Er-Pr series: 9 samples (x = 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900)
- Nd-Pr series:我们发现 9 samples (x = 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900)
- Additional B-C-O-Cu-Sr samples: 6 samples (seem unrelated to RE-Pr system)

Total instrument time: 30 units

## Measurements
I'll start by measuring the Er and Nd samples to establish the trend.

### Er-Pr Series Measurements
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
2. O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
3. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
4. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
5. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
6. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
7. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
8. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K (non-superconducting)
9. O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (non-superconducting)

Observations: Tc decreases monotonically with increasing Pr content. Tc reaches 0 at x=0.8.

### Nd-Pr Series Measurements
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
2. O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
3. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
4. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
5. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
6. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
7. O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K (non-superconducting)
8. O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K (non-superconducting)
9. O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K (non-superconducting)
10. O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K (non-superconducting)

Observations: For Nd, Tc decreases more rapidly with Pr content. Tc is already very low (5.6 K) at x=0.3 and becomes 0 at x=0.5.

### B-C-O-Cu-Sr System Measurements (additional context)
1. B0.040C0.960O5Cu1Sr2: Tc = 7.0 K
2. B0.050C0.950O5Cu1Sr2: Tc = 12.5 K
3. B0.060C0.940O5Cu1Sr2: Tc = 21.0 K
4. B0.070C0.930O5Cu1Sr2: Tc = 23.0 K
5. B0.080C0.920O5Cu1Sr2: Tc = 25.0 K
6. B0.090C0.910O5Cu1Sr2: Tc = 26.5 K

Observations: In this system, Tc increases with B doping, opposite to the Pr doping effect in RE-Pr-Ba-Cu-O. This shows doping effects are system-specific: Pr suppresses Tc in RE-123, while B enhances Tc in this B-C-O-Cu-Sr system.

### Analysis of RE-Pr-Ba-Cu-O System
Key observations:
1. For both Er and Nd, Tc decreases with increasing Pr content (x)
2. The suppression is stronger for Nd than for Er
3. Critical Pr content where Tc = 0:
   - Er: x_critical ≈ 0.8 (between x=0.5 and x=0.8)
   - Nd: x_critical ≈ 0.5 (between x=0.3 and x=0.5)
4. Base Tc (x=0.03 approximate pure RE):
   - Er: ~93.9 K
   - Nd: ~85.1 K
   
The data suggests Gd will have its own characteristic suppression curve.

### Quantitative Analysis

**Model**: Tc(RE, x) = Tc0(RE) × (1 - x / x_critical(RE))
Where:
- Tc0(RE) = Tc of pure REBa2Cu3O7 (x≈0)
- x_critical(RE) = Pr content where Tc = 0

**Parameter estimation from data**:

For Er:
- Tc0(Er) ≈ 93.9 / (1 - 0.03/x_critical) = 97.6K (if x_critical=0.8)
- From data: Tc reaches 0 between x=0.5 and 0.8, so x_critical(Er) ≈ 0.65-0.75

For Nd:
- Tc0(Nd) ≈ 85.1 / (1 - 0.03/x_critical) = 90.5K (if x_critical=0.5)
- From data: Tc reaches 0 between x=0.3 and 0.5, so x_critical(Nd) ≈ 0.4-0.5

**Pattern**: Smaller RE ion (Er³⁺: 1.00Å) tolerates more Pr than larger RE ion (Nd³⁺: 1.12Å)
Gd³⁺ ionic radius: 1.05Å (intermediate between Er and Nd)

**Estimate for Gd**:
- Ionic radius trend suggests x_critical(Gd) between 0.5 (Nd) and 0.75 (Er), perhaps ~0.625
- Tc0(Gd): GdBa2Cu3O7 typically has Tc ~92-94K, similar to other RE-123
- I'll estimate: Tc0(Gd) = 93K, x_critical(Gd) = 0.65

**Prediction formula for Gd**:
Tc(Gd, x) = 93 × (1 - x/0.65) for x < 0.65, else 0

### Final Predictions for Target Compositions

| Composition | x | Predicted Tc (K) |
|-------------|---|------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 88.7 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 81.8 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 71.5 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 64.3 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 50.1 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 35.8 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 21.5 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 7.2 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0.0 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 |

**Best composition**: O7Cu3Ba2Pr0.030Gd0.970 (highest Tc = 88.7 K)
**Critical Pr content**: 0.600 (highest x with Tc > 0)

### Law Statement
The critical temperature Tc in RE(1-x)Pr(x)Ba2Cu3O7 decreases linearly with Pr content x as Tc = Tc0(RE) × (1 - x/x_critical(RE)), where Tc0 is the base Tc of the pure RE compound and x_critical is the Pr content where superconductivity vanishes. For Gd, Tc0 ≈ 93 K and x_critical ≈ 0.65, based on interpolation between Er (smaller ion, x_critical ≈ 0.75) and Nd (larger ion, x_critical ≈ 0.5).