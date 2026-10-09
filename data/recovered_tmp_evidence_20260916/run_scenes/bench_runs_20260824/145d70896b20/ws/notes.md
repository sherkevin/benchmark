# Notes: Predicting Tc for Gd-series cuprates

## Task
Predict the critical temperature (Tc) for all Gd-series compositions in the materials database. The Gd compositions have not been synthesized, so their Tc cannot be measured directly.

## Database Discovery
Used `search_db` to find all compositions containing Pr, Er, Nd, and Gd.

**Gd-series compositions (11 total, all unsynthesized):**
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

## Measurements
Measured all synthesized Er and Nd compositions to establish the Tc vs Pr relationship for each rare earth.

**Er series Tc measurements:**
| Composition | Pr content | Tc (K) |
|-------------|------------|--------|
| O7Cu3Ba2Pr0.030Er0.970 | 0.030 | 93.9 |
| O7Cu3Ba2Pr0.076Er0.924 | 0.076 | 91.1 |
| O7Cu3Ba2Pr0.150Er0.850 | 0.150 | 82.9 |
| O7Cu3Ba2Pr0.200Er0.800 | 0.200 | 76.4 |
| O7Cu3Ba2Pr0.300Er0.700 | 0.300 | 61.1 |
| O7Cu3Ba2Pr0.400Er0.600 | 0.400 | 45.8 |
| O7Cu3Ba2Pr0.500Er0.500 | 0.500 | 28.6 |
| O7Cu3Ba2Pr0.800Er0.200 | 0.800 | 0.0 |
| O7Cu3Ba2Pr0.900Er0.100 | 0.900 | 0.0 |

**Nd series Tc measurements:**
| Composition | Pr content | Tc (K) |
|-------------|------------|--------|
| O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | 85.1 |
| O7Cu3Ba2Pr0.080Nd0.920 | 0.080 | 68.2 |
| O7Cu3Ba2Pr0.150Nd0.850 | 0.150 | 40.7 |
| O7Cu3Ba2Pr0.200Nd0.800 | 0.200 | 31.4 |
| O7Cu3Ba2Pr0.300Nd0.700 | 0.300 | 5.6 |
| O7Cu3Ba2Pr0.500Nd0.500 | 0.500 | 0.0 |
| O7Cu3Ba2Pr0.600Nd0.400 | 0.600 | 0.0 |
| O7Cu3Ba2Pr0.700Nd0.300 | 0.700 | 0.0 |
| O7Cu3Ba2Pr0.800Nd0.200 | 0.800 | 0.0 |
| O7Cu3Ba2Pr0.900Nd0.100 | 0.900 | 0.0 |

## Pattern Analysis
- Er series: Tc decreases gradually with increasing Pr, reaching 0 at Pr≈0.800
- Nd series: Tc decreases much more rapidly, reaching 0 at Pr≈0.500
- Nd suppresses Tc more strongly than Er

## Reasoning for Gd Prediction
Gadolinium (Gd, Z=64) lies exactly midway between Neodymium (Nd, Z=60) and Erbium (Er, Z=68) in the lanthanide series. Therefore, the Tc suppression behavior of Gd should be intermediate between Nd and Er.

**Prediction method:** Linear interpolation between Er and Nd Tc curves at each Pr concentration. For each Gd composition:
1. Interpolate Er Tc at the given Pr content
2. Interpolate Nd Tc at the given Pr content
3. Take the average (since Gd is midway in atomic number)

## Predicted Tc Values for Gd Series
| Composition | Pr | Er Tc (interp) | Nd Tc (interp) | Gd Tc (avg) |
|-------------|-----|----------------|----------------|-------------|
| Pr0.030Gd0.970 | 0.030 | 93.9 | 85.1 | 89.5 |
| Pr0.078Gd0.922 | 0.078 | 90.9 | 68.9 | 79.9 |
| Pr0.150Gd0.850 | 0.150 | 82.9 | 40.7 | 61.8 |
| Pr0.200Gd0.800 | 0.200 | 76.4 | 31.4 | 53.9 |
| Pr0.300Gd0.700 | 0.300 | 61.1 | 5.6 | 33.4 |
| Pr0.400Gd0.600 | 0.400 | 45.8 | 2.8 | 24.3 |
| Pr0.500Gd0.500 | 0.500 | 28.6 | 0.0 | 14.3 |
| Pr0.600Gd0.400 | 0.600 | 19.1 | 0.0 | 9.5 |
| Pr0.700Gd0.300 | 0.700 | 9.5 | 0.0 | 4.8 |
| Pr0.800Gd0.200 | 0.800 | 0.0 | 0.0 | 0.0 |
| Pr0.900Gd0.100 | 0.900 | 0.0 | 0.0 | 0.0 |

## Verification
Confirmed Gd compositions cannot be measured (sample_available: false) as expected for unsynthesized materials.
