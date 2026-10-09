# Notes: Predicting Tc for Gd-series cuprates

## Task Summary
Predict the critical temperature (Tc) of Gd(1-x)Pr(x)Ba2Cu3O7 compositions that have not been synthesized yet.

## Step 1: Discovered compositions via search_db

**Gd series (11 compositions - NOT synthesized, need prediction):**
- O7Cu3Ba2Pr0.030Gd0.970 (x=0.030)
- O7Cu3Ba2Pr0.078Gd0.922 (x=0.078)
- O7Cu3Ba2Pr0.150Gd0.850 (x=0.150)
- O7Cu3Ba2Pr0.200Gd0.800 (x=0.200)
- O7Cu3Ba2Pr0.300Gd0.700 (x=0.300)
- O7Cu3Ba2Pr0.400Gd0.600 (x=0.400)
- O7Cu3Ba2Pr0.500Gd0.500 (x=0.500)
- O7Cu3Ba2Pr0.600Gd0.400 (x=0.600)
- O7Cu3Ba2Pr0.700Gd0.300 (x=0.700)
- O7Cu3Ba2Pr0.800Gd0.200 (x=0.800)
- O7Cu3Ba2Pr0.900Gd0.100 (x=0.900)

## Step 2: Measured Tc for Nd and Er series

**Nd series data:**
| Pr_x | Tc (K) |
|------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600 | 0.0 |
| 0.700 | 0.0 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

**Er series data:**
| Pr_x | Tc (K) |
|------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

## Step 3: Analysis and Pattern

Gd (atomic number 64) lies between Nd (60) and Er (68) in the lanthanide series.
- Nd series: Tc suppressed rapidly, reaches 0 at ~50% Pr
- Er series: Tc suppressed more slowly, reaches 0 at ~80% Pr

Gd should show intermediate behavior. Since Gd is roughly midway between Nd and Er, I interpolated Tc values as the average of Nd and Er at each Pr content.

## Step 4: Predictions for Gd series (interpolating between Nd and Er)

For each Gd composition, I matched Pr content and averaged Nd/Er Tc:

| Gd Composition | Pr_x | Nd Tc | Er Tc | Gd Prediction |
|----------------|------|-------|-------|---------------|
| Pr0.030Gd0.970 | 0.030 | 85.1 | 93.9 | (85.1+93.9)/2 = **89.5** |
| Pr0.078Gd0.922 | 0.078 | 68.2* | 91.1* | (68.2+91.1)/2 = **79.7** |
| Pr0.150Gd0.850 | 0.150 | 40.7 | 82.9 | (40.7+82.9)/2 = **61.8** |
| Pr0.200Gd0.800 | 0.200 | 31.4 | 76.4 | (31.4+76.4)/2 = **53.9** |
| Pr0.300Gd0.700 | 0.300 | 5.6 | 61.1 | (5.6+61.1)/2 = **33.4** |
| Pr0.400Gd0.600 | 0.400 | 0.0 | 45.8 | (0+45.8)/2 = **22.9** |
| Pr0.500Gd0.500 | 0.500 | 0.0 | 28.6 | (0+28.6)/2 = **14.3** |
| Pr0.600Gd0.400 | 0.600 | 0.0 | ~19* | (0+19)/2 = **9.6** |
| Pr0.700Gd0.300 | 0.700 | 0.0 | ~9.5* | (0+9.5)/2 = **4.8** |
| Pr0.800Gd0.200 | 0.800 | 0.0 | 0.0 | **0.0** |
| Pr0.900Gd0.100 | 0.900 | 0.0 | 0.0 | **0.0** |

*Interpolated Er values between measured points

## Final Predictions (written to answer.json):
- O7Cu3Ba2Pr0.030Gd0.970: 89.5 K
- O7Cu3Ba2Pr0.078Gd0.922: 79.7 K
- O7Cu3Ba2Pr0.150Gd0.850: 61.8 K
- O7Cu3Ba2Pr0.200Gd0.800: 53.9 K
- O7Cu3Ba2Pr0.300Gd0.700: 33.4 K
- O7Cu3Ba2Pr0.400Gd0.600: 22.9 K
- O7Cu3Ba2Pr0.500Gd0.500: 14.3 K
- O7Cu3Ba2Pr0.600Gd0.400: 9.6 K
- O7Cu3Ba2Pr0.700Gd0.300: 4.8 K
- O7Cu3Ba2Pr0.800Gd0.200: 0.0 K
- O7Cu3Ba2Pr0.900Gd0.100: 0.0 K

**Reasoning:** Gd lies between Nd and Er in the lanthanide series, so its Tc vs Pr behavior is interpolated as the average of the two measured series at each Pr concentration.
