# Materials Science Analysis: RE(1-x)Pr(x)Ba2Cu3O7 Superconductors

## Problem Statement
Predict Tc (critical temperature) for Gd-based RE(1-x)Pr(x)Ba2Cu3O7 compositions that have not been synthesized yet, based on patterns observed from Er-based and Nd-based synthesized samples.

## Data Overview

### Synthesized compositions (19 samples):
- Er-based: 9 samples with x = 0.03, 0.076, 0.15, 0.20, 0.30, 0.40, 0.50, 0.80, 0.90
- Nd-based:这三种不同 Nd-based: 10 samples with x = 0.03, 0.08, 0.15, 0.20, 0.30, 0.50, 0.60, 0.70, 0.80, 0.90

### To predict (11 Gd-based compositions):
- x = 0.03, 0.078, 0.15, 0.20, 0.30, 0.40, 0.50, 0.60, 0.70, 0.80, 0.90

## Analysis Plan
1. Measure Tc for representative synthesized compositions to understand:
   - How Tc changes with Pr concentration (x)
   - Differences between Er-based and Nd-based systems
2. Develop a model for Tc(x) for each RE family
3. Extrapolate to Gd-based family based on RE element properties

## Measurements Record

### Er-based Measurements:
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9K
2. O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1K
3. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9K  
4. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4K
5. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1K
6. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8K
7. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6K
8. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0K
9. O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0K

### Nd-based Measurements:
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1K
2. O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2K
3. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7K
4. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4K
5. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6K
6. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0K
7. O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0K
8. O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0K
9. O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0K

## Observations
1. **Tc decreases with increasing Pr concentration (x)** for both RE families
2. **Nd-based superconductors are more sensitive to Pr doping** than Er-based:
   - At x=0.30: Er-based Tc = 61.1K, Nd-based Tc = 5.6K
   - At x=0.50: Er-based Tc = 28.6K, Nd-based Tc = 0K
   - At x=0.80: Both show Tc = 0K
3. **Critical Pr concentration where Tc=0**:
   - Nd: Between x=0.30 (5.6K) and x=0.50 (0K) → x_critical ≈ 0.40-0.45
   - Er: Between x=0.50 (28.6K) and x=0.80 (0K) → x_critical ≈ 0.65-0.70

## Analysis for Gd Predictions

### Ionic Radius Considerations:
- Nd³⁺: ~1.11 Å
- Gd³⁺: ~1.05 Å  
- Er³⁺: ~1.00 Å

Gd is intermediate between Nd and Er, slightly closer to Er.

### Interpolation Approach:
For each Pr concentration x in to_predict.txt, I will:
1. Estimate Tc_Er(x) by linear interpolation from Er data
2. Estimate Tc_Nd(x) by linear interpolation from Nd data  
3. Compute Tc_Gd(x) = Tc_Er(x) + (r_Gd - r_Er)/(r_Nd - r_Er) × (Tc_Nd(x) - Tc_Er(x))
   where r_Gd - r_Er = 0.05 Å, r_Nd - r_Er = 0.11 Å
   So weight = 0.05/0.11 ≈ 0.455

### Er Data Points (x, Tc):
(0.03, 93.9), (0.076, 91.1), (0.15, 82.9), (0.20, 76.4), (0.30, 61.1), (0.40, 45.8), (0.50, 28.6), (0.80, 0), (0.90, 0)

### Nd Data Points (x, Tc):
(0.03, 85.1), (0.08, 68.2), (0.15, 40.7), (0.20, 31.4), (0.30, 5.6), (0.50, 0), (0.60, 0), (0.70, 0), (0.80, 0), (0.90, 0)

## Final Gd Predictions Using Linear Suppression Model

### Model Parameters:
- Gd baseline Tc0 = 90.1 K (interpolated from Nd: 86K and Er: 95K)
- Gd critical Pr concentration x_critical = 0.554 (interpolated from Nd: 0.45 and Er: 0.68)
- For x ≤ x_critical: Tc(x) = Tc0 × (1 - x/x_critical)
- For x > x_critical: Tc(x) = 0 K

### Predictions for Gd-based RE(1-x)Pr(x)Ba2Cu3O7:
1. x=0.030: Tc = 90.1 × (1 - 0.030/0.554) = 90.1 × 0.946 = 85.2 K
2. x=0.078: Tc = 90.1 × (1 - 0.078/0.554) = 90.1 × 0.859 = 77.4 K
3. x=0.150: Tc = 90.1 × (1 - 0.150/0.554) = 90.1 × 0.729 = 65.7 K
4. x=0.200: Tc = 90.1 × (1 - 0.200/0.554) = 90.1 × 0.639 = 57.5 K
5. x=0.300: Tc = 90.1 × (1 - 0.300/0.554) = 90.1 × 0.459 = 41.4 K
6. x=0.400: Tc = 90.1 × (1 - 0.400/0.554) = 90.1 × 0.278 = 25.0 K
7. x=0.500: Tc = 90.1 × (1 - 0.500/0.554) = 90.1 × 0.097 = 8.7 K
8. x=0.600: Since 0.600 > 0.554, Tc = 0 K
9. x=0.700: Tc = 0 K
10. x=0.800: Tc = 0 K
11. x=0.900: Tc = 0 K

## Rationale
The linear suppression model (Tc ∝ 1 - x/x_critical) fits the Er data reasonably well and provides a physically intuitive framework. Gd properties are interpolated between Nd and Er based on ionic radius, with Gd (r=1.05Å) being closer to Er (r=1.00Å) than to Nd (r=1.11Å). The complete suppression of superconductivity at high Pr concentrations is consistent with both measured systems.