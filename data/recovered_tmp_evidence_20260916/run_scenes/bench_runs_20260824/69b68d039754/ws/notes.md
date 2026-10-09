# Notes: Predicting Tc for Gd-series compositions

## Task
Predict the critical temperature (Tc) for all 11 Gd-series compositions that have not been synthesized:
- O7Cu3Ba2Pr0.030Gd0.970
- O7Cu3Ba2Pr0.078Gd0.922
- O7Cu3Ba2Pr0.150Gd0.850
- O7Cu3Ba2Pr0.200Gd0.800
- O7Cu3Ba2Pr0.300Gd0.700
- O7Cu3Ba2Pr0.400Gd0.600
- O7Cu3Ba2Pr0.500Gd0.500
- O7Cu3Ba2Pr0.600Gd0.400
- O7Cu3Ba2Pr0.700Gd0.300
- O7Cu3Ba2Pr0.800Gd0.200
- O7Cu3Ba2Pr0.900Gd0.100

## Database Discovery
Found 30 compositions with Pr, including:
- 11 Gd-series (not synthesized - cannot measure)
- 10+ Er-series 
- 8+ Nd-series

## Strategy
1. Measure Tc for Er and Nd series to understand how Tc depends on:
   - Pr concentration (x)
   - The rare earth element (Er vs Nd)
2. Extrapolate to predict Gd series behavior

## Measurements Collected

### Er series Tc values:
| Pr (x) | Tc (K) |
|--------|--------|
| 0.030  | 93.9   |
| 0.076  | 91.1   |
| 0.150  | 82.9   |
| 0.200  | 76.4   |
| 0.300  | 61.1   |
| 0.400  | 45.8   |
| 0.500  | 28.6   |
| 0.800  | 0.0    |
| 0.900  | 0.0    |

### Nd series Tc values:
| Pr (x) | Tc (K) |
|--------|--------|
| 0.030  | 85.1   |
| 0.080  | 68.2   |
| 0.150  | 40.7   |
| 0.200  | 31.4   |
| 0.300  | 5.6    |
| 0.500  | 0.0    |
| 0.600  | 0.0    |
| 0.700  | 0.0    |
| 0.800  | 0.0    |
| 0.900  | 0.0    |

## Analysis

Er retains superconductivity much better than Nd as Pr increases:
- Er stays superconducting up to x=0.5 (28.6 K), drops to 0 at x=0.8
- Nd drops to near-zero by x=0.3 (5.6 K) and is zero at x=0.5+

Gd (atomic number 64) is between Nd (60) and Er (68) in the lanthanide series.
Therefore, Gd's Tc suppression should be intermediate between Nd and Er.

## Prediction Strategy

Use linear interpolation between Nd and Er Tc values at each Pr concentration:
Gd_Tc ≈ (Nd_Tc + Er_Tc) / 2

For Pr levels where one series is missing data, interpolate from nearby points.
For high Pr (x ≥ 0.7), both series approach 0, so Gd should also be 0.

## Predictions for Gd series

| Gd Composition | Pr (x) | Nd Tc | Er Tc | Gd Prediction |
|----------------|--------|-------|-------|---------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 85.1 | 93.9 | (85.1+93.9)/2 = 89.5 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | ~68.2 | ~91.1 | ~79.7 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 40.7 | 82.9 | 61.8 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 31.4 | 76.4 | 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 5.6 | 61.1 | 33.4 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | ~0* | 45.8 | ~23 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 0.0 | 28.6 | 14.3 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0.0 | ~15* | ~7.5 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0.0 | ~40* | ~20 → but Nd is 0, likely lower |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 | 0.0 | 0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 | 0.0 | 0 |

* interpolated values

## Final Calculations

Er interpolation for missing points (linear between x=0.5 and x=0.8):
- x=0.6: 28.6 - (28.6/0.3)*0.1 = 19.1 K
- x=0.7: 28.6 - (28.6/0.3)*0.2 = 9.5 K

Nd interpolation for x=0.4 (linear between x=0.3 and x=0.5):
- x=0.4: 5.6 - (5.6/0.2)*0.1 = 2.8 K

## Final Gd Predictions (average of Nd and Er at each Pr level)

| Gd Composition | Pr (x) | Nd Tc | Er Tc | Gd Prediction |
|----------------|--------|-------|-------|---------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 85.1 | 93.9 | 89.5 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 68.2 | 91.1 | 79.7 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 40.7 | 82.9 | 61.8 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 31.4 | 76.4 | 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 5.6 | 61.1 | 33.4 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 2.8 | 45.8 | 24.3 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 0.0 | 28.6 | 14.3 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0.0 | 19.1 | 9.6 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0.0 | 9.5 | 4.8 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 | 0.0 | 0.0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 | 0.0 | 0.0 |

## Reasoning Summary

Gd lies between Nd and Er in the lanthanide series (atomic numbers 60, 64, 68). 
Since Tc suppression by Pr is strongest for Nd and weakest for Er, Gd should show 
intermediate behavior. I predicted Gd Tc values by averaging the measured Tc of 
Nd and Er series at each Pr concentration, with linear interpolation for missing data points.
