# Notes: Predicting Tc for Gd-Pr-Ba-Cu-O series

## Measurements Made (14 units used)

### Erbium (Er) series:
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
2. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
3. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
4. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
5. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Neodymium (Nd) series:
6. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
7. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
8. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
9. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
10. O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

### B/C series (reference material):
11. B0.040C0.960O5Cu1Sr2: Tc = 7.0 K
12. B0.060C0.940O5Cu1Sr2: Tc = 21.0 K
13. B0.070C0.930O5Cu1Sr2: Tc = 23.0 K
14. B0.090C0.910O5Cu1Sr2: Tc = 26.5 K

## Observed Trends

1. **Pr content effect**: For both Er and Nd series, Tc decreases monotonically with increasing Pr content (x).

2. **Rare earth element effect**: At the same Pr content, Er (atomic number 68) consistently shows higher Tc than Nd (atomic number 60).

3. **Critical Pr content**: 
   - Er loses superconductivity between x=0.500 (28.6K) and x=0.800 (0K)
   - Nd loses superconductivity between x=0.300 (5.6K) and x=0.500 (0K)

4. **Non-linear suppression**: The suppression of Tc by Pr doping appears non-linear - the effect becomes more pronounced at higher Pr content.

## Relationship Inference

Based on the data, I observe that:
1. Tc decreases with increasing Pr content for all rare earth elements
2. The rate of Tc suppression depends on the rare earth element
3. Gd (atomic number 64) lies between Nd (60) and Er (68) in the periodic table

I hypothesize that Gd's behavior will be intermediate between Nd and Er. To quantify this, I can:
1. For each Pr content x, interpolate between Nd and Er Tc values based on atomic number
2. Account for the non-linear dependence on Pr content

## Prediction Strategy

I use a two-step approach:
1. For measured x values (0.030, 0.150, 0.300, 0.500, 0.800), estimate Tc(Gd) as weighted average of Tc(Er) and Tc(Nd)
2. For intermediate x values, use piecewise linear interpolation between these estimated points

Weighted average: Gd atomic number 64 is midway between Nd (60) and Er (68), so weight = 0.5 for each.

Estimated Tc(Gd) at measured x:
- x=0.030: 0.5×93.9 + 0.5×85.1 = 89.5 K
- x=0.150: 0.5×82.9 + 0.5×40.7 = 61.8 K  
- x=0.300: 0.5×61.1 + 0.5×5.6 = 33.35 K
- x=0.500: 0.5×28.6 + 0.5×0.0 = 14.3 K
- x=0.800: 0.5×0.0 + 0.5×0.0* = 0.0 K (*assuming Nd Tc=0 at x=0.800)
- x=0.900: 0.5×0.0* + 0.5×0.0 = 0.0 K (*assuming Er Tc=0 at x=0.900)

## Detailed Predictions

Using piecewise linear interpolation between estimated points:

1. x=0.078 (between 0.030 and 0.150):
   Tc = 89.5 + (61.8-89.5)×(0.078-0.030)/(0.15-0.03) = 89.5 - 27.7×0.048/0.12 = 78.42 K

2. x=0.200 (between 0.150 and 0.300):
   Tc = 61.8 + (33.35-61.8)×(0.20-0.15)/(0.30-0.15) = 61.8 - 28.45×0.05/0.15 = 52.32 K

3. x=0.400 (between 0.300 and 0.500):
   Tc = 33.35 + (14.3-33.35)×(0.40-0.30)/(0.50-0.30) = 33.35 - 19.05×0.10/0.20 = 23.83 K

4. x=0.600 (between 0.500 and 0.800):
   Tc = 14.3 + (0-14.3)×(0.60-0.50)/(0.80-0.50) = 14.3 - 14.3×0.10/0.30 =775 K (correcting: 14.3 - 4.77 = 9.53 K)

5. x=0.700 (between 0.500 and 0.800):
   Tc = 14.3 + (0-14.3)×(0.70-0.50)/(0.80-0.50) = 14.3 - 14.3×0.20/0.30 = 4.77 K

## Critical Points Calculation

**max_pr_above_80K**: Highest x with Tc ≥ 80K
- x=0.030: 89.5K ✓
- x=0.078: 78.42K ✗ (<80K)
Thus max_pr_above_80K = 0.030

**critical_pr**: Highest x with Tc > 0K
- x=0.700: 4.77K ✓  
- x=0.800: 0K ✗
Thus critical_pr = 0.700

Note: The linear model predicts Tc=0 exactly at x=0.800. In reality, the transition might occur slightly below 0.800, but 0.700 is conservative.

## Summary

All 14 instrument time units were used to measure:
- 5 Er-Pr-Ba-Cu-O samples covering x=0.030 to 0.800
- 5 Nd-Pr-Ba-Cu-O samples covering x=0.030 to 0.900
- 4 B-C-Sr-Cu-O reference samples (showing different behavior)

The data shows Tc decreases with increasing Pr content for both Er and Nd, with Nd showing stronger suppression. Gd's behavior was estimated as intermediate using weighted averaging (0.5 weight each) at measured x values, with piecewise linear interpolation for intermediate x.

Final predictions for all 11 Gd target compositions are in answer.json, along with:
- max_pr_above_80K = 0.030 (only x=0.030 maintains Tc ≥ 80K)
- critical_pr = 0.700 (superconductivity persists up to x≈0.700)