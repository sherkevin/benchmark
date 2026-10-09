# Notes: Predicting Tc of Gd-series RE(1-x)Pr(x)Ba2Cu3O7

## 1. Database Discovery

Searched for all compositions in the RE(1-x)Pr(x)Ba2Cu3O7 family. Found three RE series:
- **Er** (9 compositions): Pr from 0.030 to 0.900
- **Nd** (10 compositions): Pr from 0.030 to 0.900
- **Gd** (11 compositions): Pr from 0.030 to 0.900 — **not synthesized**

No other RE elements (Y, Sm, Eu, Dy, Ho, La, Tm, Lu, Tb) found in the database.

## 2. Measured Tc Data

### Er series (synthesized)
| x(Pr) | Tc (K) |
|-------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

### Nd series (synthesized)
| x(Pr) | Tc (K) |
|-------|--------|
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

### Key observations:
- Pr substitution suppresses Tc in both series
- Nd series has much steeper suppression (reaches 0 at x~0.32) vs Er (x~0.64)

## 3. Interpolation Strategy

Shannon ionic radii (RE3+, CN=8): Er=1.004, Gd=1.053, Nd=1.109 Å

Interpolation fraction: f = (1.053-1.004)/(1.109-1.004) = 0.467
Tc_Gd(x) = 0.533*Tc_Er(x) + 0.467*Tc_Nd(x)

## 4. Predictions
- x=0.030: 0.533*93.9 + 0.467*85.1 = 89.8
- x=0.078: 0.533*91.0 + 0.467*68.9 = 80.7
- x=0.150: 0.533*82.9 + 0.467*40.7 = 63.2
- x=0.200: 0.533*76.4 + 0.467*31.4 = 55.4
- x=0.300: 0.533*61.1 + 0.467*5.6 = 35.2
- x=0.400: 0.533*45.8 + 0.467*0 = 24.4 (Nd at 0)
- x=0.500: 0.533*28.6 + 0.467*0 = 15.2
- x=0.600: 0.533*9.7 + 0.467*0 = 5.2 (Er from quadratic extrapolation)
- x=0.700: 0
- x=0.800: 0
- x=0.900: 0
