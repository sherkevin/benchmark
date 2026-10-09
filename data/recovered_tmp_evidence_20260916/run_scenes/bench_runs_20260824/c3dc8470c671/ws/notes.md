# Notes: Predicting the Tc curve of the Gd(1-x)Pr(x)Ba2Cu3O7 series

## 1. Discovery

Searched the database for compositions containing Gd, Pr, Er, and Nd. Found three RE series:
- **Er series** (9 compositions): Pr from 0.030 to 0.900
- **Nd series** (10 compositions): Pr from 0.030 to 0.900
- **Gd series** (11 compositions, NOT synthesized): Pr from 0.030 to 0.900

No pure REBa2Cu3O7 end-members in the database.

## 2. Measured Tc values

### Er series (synthesized):
| Pr content (x) | Tc (K) |
|-----------------|--------|
| 0.030           | 93.9   |
| 0.076           | 91.1   |
| 0.150           | 82.9   |
| 0.200           | 76.4   |
| 0.300           | 61.1   |
| 0.400           | 45.8   |
| 0.500           | 28.6   |
| 0.800           | 0.0    |
| 0.900           | 0.0    |

### Nd series (synthesized):
| Pr content (x) | Tc (K) |
|-----------------|--------|
| 0.030           | 85.1   |
| 0.080           | 68.2   |
| 0.150           | 40.7   |
| 0.200           | 31.4   |
| 0.300           | 5.6    |
| 0.500           | 0.0    |
| 0.600           | 0.0    |
| 0.700           | 0.0    |
| 0.800           | 0.0    |
| 0.900           | 0.0    |

### Gd series: Confirmed not synthesized (sample_available: false)

## 3. Pattern analysis

Key observations:
1. **Er series**: Tc starts at ~94 K and decreases gradually with Pr content. Tc goes to zero between x=0.5 and x=0.8 (critical Pr concentration x_c ~ 0.65-0.70).
2. **Nd series**: Tc starts at ~85 K and decreases much more rapidly. Tc goes to zero between x=0.3 and x=0.5 (x_c ~ 0.32-0.35).
3. Pr suppresses Tc much more strongly in Nd than in Er — consistent with the known physics that larger RE ions (closer in size to Pr) suffer stronger suppression.

Ionic radii (8-coordination, Shannon):
- Nd3+: 1.109 A
- Gd3+: 1.053 A
- Er3+: 1.004 A

By atomic number: Nd (60), Gd (64), Er (68) — Gd is exactly midway.
By ionic radius: Gd is 47% of the way from Er to Nd — very close to the midpoint.

## 4. Interpolation method

Since Gd (Z=64) lies midway between Nd (Z=60) and Er (Z=68), I predict:

**Tc_Gd(x) = 0.5 * Tc_Er(x) + 0.5 * Tc_Nd(x)**

At each Gd composition's Pr content, I interpolated the Er and Nd Tc values (using piecewise linear interpolation of measured data, and extrapolated Er beyond x=0.5 using the observed curvature/slope trend), then averaged.

For Er beyond x=0.5: The rate of Tc decrease accelerates with x. Extrapolating the slope trend gives Tc_Er(0.6) ~ 10 K and Tc_Er(0.7) ~ 0 K.

For Nd beyond x=0.3: Tc is near zero; used linear interpolation between (0.3, 5.6) and (0.5, 0) for intermediate x values.

## 5. Predicted Gd Tc values

| Composition                        | x     | Er_Tc | Nd_Tc | Predicted Gd_Tc |
|------------------------------------|-------|-------|-------|-----------------|
| O7Cu3Ba2Pr0.030Gd0.970            | 0.030 | 93.9  | 85.1  | 89.5            |
| O7Cu3Ba2Pr0.078Gd0.922            | 0.078 | 90.9  | 68.9  | 79.9            |
| O7Cu3Ba2Pr0.150Gd0.850            | 0.150 | 82.9  | 40.7  | 61.8            |
| O7Cu3Ba2Pr0.200Gd0.800            | 0.200 | 76.4  | 31.4  | 53.9            |
| O7Cu3Ba2Pr0.300Gd0.700            | 0.300 | 61.1  | 5.6   | 33.4            |
| O7Cu3Ba2Pr0.400Gd0.600            | 0.400 | 45.8  | 2.8   | 24.3            |
| O7Cu3Ba2Pr0.500Gd0.500            | 0.500 | 28.6  | 0.0   | 14.3            |
| O7Cu3Ba2Pr0.600Gd0.400            | 0.600 | ~10   | 0.0   | 5.0             |
| O7Cu3Ba2Pr0.700Gd0.300            | 0.700 | ~0    | 0.0   | 0               |
| O7Cu3Ba2Pr0.800Gd0.200            | 0.800 | 0.0   | 0.0   | 0               |
| O7Cu3Ba2Pr0.900Gd0.100            | 0.900 | 0.0   | 0.0   | 0               |

The Gd series Tc drops steadily with Pr content, going to zero around x ~ 0.65 — intermediate between the Er series (x_c ~ 0.67) and the Nd series (x_c ~ 0.33).
