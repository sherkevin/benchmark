# Notes: Predicting Tc for Gd-series cuprates

## Task
Predict the critical temperature (Tc) for all Gd-series compositions in the database. The Gd series has not been synthesized, so Tc must be inferred from other RE (rare earth) series.

## Database Discovery
Searched for compositions containing different rare earth elements:
- **Gd series**: 11 compositions (Pr content from 0.030 to 0.900)
- **Er series**: 9 compositions (synthesized, measurable)
- **Nd series**: 10 compositions (synthesized, measurable)

Formula: RE(1-x) Pr(x) Ba2 Cu3 O7, where RE = Er, Gd, or Nd

## Measurements Taken

### Er series (measured Tc in K):
| Pr content | Tc (K) |
|------------|--------|
| 0.030      | 93.9   |
| 0.076      | 91.1   |
| 0.150      | 82.9   |
| 0.200      | 76.4   |
| 0.300      | 61.1   |
| 0.400      | 45.8   |
| 0.500      | 28.6   |
| 0.800      | 0.0    |
| 0.900      | 0.0    |

### Nd series (measured Tc in K):
| Pr content | Tc (K) |
|------------|--------|
| 0.030      | 85.1   |
| 0.080      | 68.2   |
| 0.150      | 40.7   |
| 0.200      | 31.4   |
| 0.300      | 5.6    |
| 0.500+     | 0.0    |

## Pattern Analysis
- **Er** shows slower Tc suppression: Tc remains high (~93 K) at low Pr, gradually decreases, and reaches 0 around Pr=0.80
- **Nd** shows faster Tc suppression: Tc drops more rapidly and reaches 0 by Pr=0.50
- The suppression rate depends on the RE element

Gd is positioned between Nd (atomic number 60) and Er (atomic number 68) in the lanthanide series (Gd = 64). Therefore, Gd's Tc suppression behavior should be intermediate between Nd and Er.

## Extrapolation Method
Used linear interpolation between Er and Nd Tc values at each Pr concentration:
- Gd Tc ≈ (Er Tc + Nd Tc) / 2

For Pr concentrations not directly measured, linearly interpolated Er and Nd values first, then averaged.

## Predicted Gd Tc Values
| Composition | Pr content | Predicted Tc (K) |
|-------------|------------|------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 89.5 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 79.9 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 61.8 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 33.35 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 24.3 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 14.3 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 9.5 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 4.8 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 |

## Reasoning Summary
The Tc suppression by Pr substitution depends on the rare earth element. Er shows gradual suppression (Tc=0 at Pr≈0.8), while Nd shows rapid suppression (Tc=0 at Pr≈0.5). Since Gd lies midway between Nd and Er in the lanthanide series, its Tc curve was predicted by averaging the Er and Nd values at each Pr concentration, yielding intermediate suppression behavior.
