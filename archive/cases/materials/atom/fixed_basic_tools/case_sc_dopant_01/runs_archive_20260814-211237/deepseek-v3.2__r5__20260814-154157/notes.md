# Materials Analysis: Predicting Tc for Gd-Pr cuprates

## Task Overview
Predict the critical temperature (Tc) for 11 Gd-Pr compositions in the RE(1-x)Pr(x)Ba2Cu3O7 system, where RE = Gd. We have synthesized data for Er-Pr and Nd-Pr systems as reference.

## Data Structure
- Synthesized materials: 19 compositions total (10 Er-Pr, 9 Nd-Pr)
- To predict: 11 Gd-Pr compositions
- Format: O7Cu3Ba2Pr{X.XXX}RE{Y.YYY} where X + Y = 1

## Measurement Plan
I'll start by measuring a representative sample of synthesized compositions to understand:
1. How Tc varies with Pr content within each RE family
2. How Tc differs between Er and Nd families
3. Extrapolate to Gd family

## Measurements

### Initial Measurements - Er-Pr System
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
2. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
3. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
4. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
5. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
6. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
7. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K (no longer superconducting)
8. O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (no longer superconducting)

### Nd-Pr System Measurements
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
2. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
3. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
4. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
5. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K (no longer superconducting)
6. O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K (no longer superconducting)

## Analysis

### Trends Observed
1. **Er-Pr system**: Tc drops gradually with increasing Pr content
   - x=0.03: 93.9 K
   - x=0.15: 82.9 K (Δ = -11.0 K)
   - x=0.20: 76.4 K (Δ = -6.5 K from x=0.15)
   - x=0.30: 61.1 K (Δ = -15.3 K from x=0.20)
   - x=0.50: 28.6 K (Δ = -32.5 K from x=0.30)
   - x=0.80: 0 K (superconductivity lost)

2. **Nd-Pr system**: Tc drops more rapidly with Pr
   - x=0.03: 85.1 K (already lower than Er at same x)
   - x=0.15: 40.7 K (Δ = -44.4 K)
   - x=0.20: 31.4 K (Δ = -9.3 K from x=0.15)
   - x=0.30: 5.6 K (Δ = -25.8 K from x=0.20)
   - x=0.50: 0 K (superconductivity lost earlier than Er)

### Key Observations
- Nd-Pr system is more sensitive to Pr doping than Er-Pr system
- Er-Pr maintains superconductivity to higher Pr concentrations
- Both systems eventually lose superconductivity at high Pr content

## Predictive Model for Gd-Pr System

### Ionic Radius Considerations
- Er³⁺: 1.03 Å (smallest)
- Gd³⁺: 1.06 Å (intermediate)
- Nd³⁺: 1.12 Å (largest)
- Pr³⁺: 1.13 Å (very close to Nd³⁺)

The ionic radius mismatch hypothesis: Pr substitution causes less lattice distortion when replacing Nd (similar size) than when replacing Er (larger size difference). This might explain why Tc suppression is more severe in Nd-Pr than Er-Pr.

Since Gd³⁺ (1.06 Å) is closer to Er³⁺ than to Nd³⁺, we expect Gd-Pr behavior to be closer to Er-Pr than to Nd-Pr.

### Quantitative Model
I'll create a linear interpolation model between Er-Pr and Nd-Pr systems, weighted by ionic radius similarity:

At each Pr concentration x:
- Calculate distance ratios: d_Er = |r_Gd - r_Er| = 0.03 Å, d_Nd = |r_Gd - r_Nd| = 0.06 Å
- Weight: w_Er = d_Nd/(d_Er + d_Nd) = 0.06/(0.03+0.06) = 0.667
- Weight: w_Nd = d_Er/(d_Er + d_Nd) = 0.03/(0.03+0.06) = 0.333

Predicted Tc_Gd(x) = w_Er × Tc_Er(x) + w_Nd × Tc_Nd(x)

For points where we don't have direct measurements, I'll need to interpolate the Er and Nd curves first.

### Superconducting Transition Points
- **Er-Pr**: Tc > 0 for x ≤ 0.50, Tc = 0 for x ≥ 0.80
  - Transition occurs between x=0.50 (28.6 K) and x=0.80 (0 K)
  
- **Nd-Pr**: Tc > 0 for x ≤ 0.30, Tc = 0 for x ≥ 0.50
  - Transition occurs between x=0.30 (5.6 K) and x=0.50 (0 K)
  
- **Expected for Gd-Pr**: Transition likely between x=0.30 and x=0.80, closer to Er behavior

## Calculations for Gd-Pr Predictions

### Data Points Available
**Er-Pr (x = Pr content):**
- x=0.030: 93.9 K
- x=0.150: 82.9 K  
- x=0.200: 76.4 K
- x=0.300: 61.1 K
- x=0.400: 45.8 K
- x=0.500: 28.6 K
- x=0.800: 0 K
- x=0.900: 0 K

**Nd-Pr (x = Pr content):**
- x=0.030: 85.1 K
- x=0.150: 40.7 K
- x=0.200: 31.4 K
- x=0.300: 5.6 K
- x=0.500: 0 K
- x=0.600: 0 K

### Interpolation Strategy
For Gd-Pr predictions at each x:
1. Find nearest Er-Pr data points and interpolate Tc_Er(x)
2. Find nearest Nd-Pr data points and interpolate Tc_Nd(x)  
3. Weighted average: Tc_Gd(x) = 0.667 × Tc_Er(x) + 0.333 × Tc_Nd(x)

### Manual Calculations
Let me calculate predictions for each Gd-Pr composition:

**Predictions:**
1. x=0.030: Tc = 0.667×93.9 + 0.333×85.1 = 91.0 K
2. x=0.078: Tc_Er ≈ 89.5 K, Tc_Nd ≈ 67.34 K → Tc_Gd = 82.1 K
3. x=0.150: Tc = 0.667×82.9 + 0.333×40.7 = 68.9 K
4. x=0.200: Tc = 0.667×76.4 + 0.333×31.4 = 61.4 K
5. x=0.300: Tc = 0.667×61.1 + 0.333×5.6 = 42.6 K
6. x=0.400: Tc_Er = 45.8 K, Tc_Nd ≈ 2.8 K → Tc_Gd = 31.5 K
7. x=0.500: Tc = 0.667×28.6 + 0.333×0 = 19.1 K
8. x=0.600: Tc_Er ≈ 19.07 K, Tc_Nd = 0 K → Tc_Gd = 12.7 K
9. x=0.700: Tc_Er ≈ 9.53 K, Tc_Nd = 0 K → Tc_Gd = 6.4 K
10. x=0.800: Tc = 0 K
11. x=0.900: Tc = 0 K

**Summary Table:**
| Composition | Pr content (x) | Predicted Tc (K) |
|-------------|----------------|------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 91.0 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 82.1 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 68.9 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 61.4 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 42.6 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 31.5 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 19.1 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 12.7 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 6.4 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0 |

## Conclusion

I have successfully predicted Tc for all 11 Gd-Pr compositions by:
1. Measuring representative samples from synthesized Er-Pr and Nd-Pr systems
2. Observing that Nd-Pr system is more sensitive to Pr doping than Er-Pr
3. Developing a model based on ionic radius similarity: Gd³⁺ (1.06 Å) is closer to Er³⁺ (1.03 Å) than to Nd³⁺ (1.12 Å)
4. Using weighted linear interpolation: Tc_Gd(x) = 0.667 × Tc_Er(x) + 0.333 × Tc_Nd(x)
5. Creating predictions for all compositions in `data/to_predict.txt`

The predictions show that Gd-Pr system maintains superconductivity to higher Pr concentrations than Nd-Pr but loses it earlier than Er-Pr, consistent with the ionic radius hypothesis. All predictions are documented in `answer.json`.