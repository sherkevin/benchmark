# Notes: Predicting Tc for Gd-series compositions

## Task
Predict the critical temperature (Tc) for all Gd-series compositions in the RE(1-x)Pr(x)Ba2Cu3O7 cuprate family.

## Step 1: Discovered compositions

### Gd-series (11 compositions - NOT synthesized, need prediction):
| Pr content | Gd content | Composition |
|------------|------------|-------------|
| 0.030 | 0.970 | O7Cu3Ba2Pr0.030Gd0.970 |
| 0.078 | 0.922 | O7Cu3Ba2Pr0.078Gd0.922 |
| 0.150 | 0.850 | O7Cu3Ba2Pr0.150Gd0.850 |
| 0.200 | 0.800 | O7Cu3Ba2Pr0.200Gd0.800 |
| 0.300 | 0.700 | O7Cu3Ba2Pr0.300Gd0.700 |
| 0.400 | 0.600 | O7Cu3Ba2Pr0.400Gd0.600 |
| 0.500 | 0.500 | O7Cu3Ba2Pr0.500Gd0.500 |
| 0.600 | 0.400 | O7Cu3Ba2Pr0.600Gd0.400 |
| 0.700 | 0.300 | O7Cu3Ba2Pr0.700Gd0.300 |
| 0.800 | 0.200 | O7Cu3Ba2Pr0.800Gd0.200 |
| 0.900 | 0.100 | O7Cu3Ba2Pr0.900Gd0.100 |

## Step 2: Measured Tc for synthesized compositions

### Er series (Pr content → Tc):
| Pr | Er | Tc (K) |
|----|----|--------|
| 0.030 | 0.970 | 93.9 |
| 0.076 | 0.924 | 91.1 |
| 0.150 | 0.850 | 82.9 |
| 0.200 | 0.800 | 76.4 |
| 0.300 | 0.700 | 61.1 |
| 0.400 | 0.600 | 45.8 |
| 0.500 | 0.500 | 28.6 |
| 0.800 | 0.200 | 0.0 |
| 0.900 | 0.100 | 0.0 |

### Nd series (Pr content → Tc):
| Pr | Nd | Tc (K) |
|----|----|--------|
| 0.030 | 0.970 | 85.1 |
| 0.080 | 0.920 | 68.2 |
| 0.150 | 0.850 | 40.7 |
| 0.200 | 0.800 | 31.4 |
| 0.300 | 0.700 | 5.6 |
| 0.500 | 0.500 | 0.0 |
| 0.600 | 0.400 | 0.0 |
| 0.700 | 0.300 | 0.0 |
| 0.800 | 0.200 | 0.0 |
| 0.900 | 0.100 | 0.0 |

## Step 3: Pattern analysis and reasoning

Key observations:
1. Pr substitution suppresses Tc in both series - higher Pr = lower Tc
2. Nd suppresses Tc much more strongly than Er at the same Pr content
3. In the lanthanide series: Nd (Z=60) < Gd (Z=64) < Er (Z=68)
4. Gd is exactly midway between Nd and Er in atomic number

**Interpolation method**: Since Gd (Z=64) is midway between Nd (Z=60) and Er (Z=68), I predict Gd Tc as the average of Nd and Er Tc values at matched Pr content.

## Step 4: Final predictions for Gd series

Using linear interpolation: Gd_Tc(Pr) ≈ (Nd_Tc(Pr) + Er_Tc(Pr)) / 2

1. **O7Cu3Ba2Pr0.030Gd0.970**: (85.1 + 93.9) / 2 = **89.5 K**
2. **O7Cu3Ba2Pr0.078Gd0.922**: Er@0.076=91.1, Nd@0.080=68.2 → (91.1 + 68.2) / 2 = **79.65 K**
3. **O7Cu3Ba2Pr0.150Gd0.850**: (40.7 + 82.9) / 2 = **61.8 K**
4. **O7Cu3Ba2Pr0.200Gd0.800**: (31.4 + 76.4) / 2 = **53.9 K**
5. **O7Cu3Ba2Pr0.300Gd0.700**: (5.6 + 61.1) / 2 = **33.35 K**
6. **O7Cu3Ba2Pr0.400Gd0.600**: Nd@0.400≈2.8 (interp), Er@0.400=45.8 → (2.8 + 45.8) / 2 = **24.3 K**
7. **O7Cu3Ba2Pr0.500Gd0.500**: (0.0 + 28.6) / 2 = **14.3 K**
8. **O7Cu3Ba2Pr0.600Gd0.400**: Nd@0.600=0, Er@0.600≈19 (interp) → (0 + 19) / 2 = **9.5 K**
9. **O7Cu3Ba2Pr0.700Gd0.300**: Nd@0.700=0, Er@0.700≈9.5 (interp) → (0 + 9.5) / 2 = **4.75 K**
10. **O7Cu3Ba2Pr0.800Gd0.200**: (0.0 + 0.0) / 2 = **0.0 K**
11. **O7Cu3Ba2Pr0.900Gd0.100**: (0.0 + 0.0) / 2 = **0.0 K**

## Reasoning summary
Gd lies midway between Nd and Er in the lanthanide series. Since Pr substitution suppresses Tc and Nd suppresses more strongly than Er, Gd should show intermediate behavior. I predicted Tc by averaging the measured Nd and Er values at matched Pr compositions, with linear interpolation where exact matches weren't available.
