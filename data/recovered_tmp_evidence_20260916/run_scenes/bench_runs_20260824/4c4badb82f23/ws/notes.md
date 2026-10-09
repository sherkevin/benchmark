# Notes: Predicting Tc of Gd(1-x)Pr(x)Ba2Cu3O7 Series

## Database Discovery

Searched for all RE(1-x)Pr(x)Ba2Cu3O7 compositions using `search_db`. Found three RE series:
- **Er series**: 9 compositions (synthesized)
- **Nd series**: 10 compositions (synthesized)
- **Gd series**: 11 compositions (NOT synthesized — need to predict)

Confirmed Gd samples unavailable: `measure_tc("O7Cu3Ba2Pr0.030Gd0.970")` → `sample_available: false`.

No other RE series (Sm, Eu, Dy, Ho, Y) found in database.

## Measured Tc Data

### Er Series (heavier RE, smaller ionic radius, Z=68)
| x(Pr)  | Tc (K) |
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

Linear fit: Tc ≈ 102.0 - 141.3·x  (Tc→0 at x≈0.72)

### Nd Series (lighter RE, larger ionic radius, Z=60)
| x(Pr)  | Tc (K) |
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

Linear fit: Tc ≈ 91.0 - 294.4·x  (Tc→0 at x≈0.31)

## Key Observation

Pr suppresses Tc much faster in Nd than in Er. The suppression rate (|slope|) correlates with the RE ionic radius — larger RE ions lead to faster Tc suppression. This is a well-known trend in RE(1-x)Pr(x)Ba2Cu3O7 cuprates.

## Interpolation Strategy

Gd is between Nd and Er in the lanthanide series. Using Shannon ionic radii (8-coordinate, 3+):
- Nd3+: 1.109 A (Z=60)
- Gd3+: 1.053 A (Z=64)
- Er3+: 1.004 A (Z=68)

Interpolation fraction: f = (r_Gd - r_Er)/(r_Nd - r_Er) = (1.053-1.004)/(1.109-1.004) = 0.049/0.105 = 0.467

So Gd is ~46.7% of the way from Er toward Nd.

Formula: Tc_Gd(x) = (1-f)*Tc_Er(x) + f*Tc_Nd(x) = 0.533*Tc_Er(x) + 0.467*Tc_Nd(x)

Where possible, used directly measured Tc values for Er and Nd. Where a series lacked a measurement at the exact x, used linear interpolation between adjacent measured points (for Nd at x=0.40) or the linear fit (for Er at x=0.60 and x=0.70).

## Predictions for Gd Series

| Composition                   | x(Pr) | Tc_Er(x) | Tc_Nd(x) | Predicted Tc_Gd |
|-------------------------------|-------|----------|----------|-----------------|
| O7Cu3Ba2Pr0.030Gd0.970       | 0.030 | 93.9     | 85.1     | 89.8            |
| O7Cu3Ba2Pr0.078Gd0.922       | 0.078 | 91.1*    | 68.2*    | 80.4            |
| O7Cu3Ba2Pr0.150Gd0.850       | 0.150 | 82.9     | 40.7     | 63.2            |
| O7Cu3Ba2Pr0.200Gd0.800       | 0.200 | 76.4     | 31.4     | 55.4            |
| O7Cu3Ba2Pr0.300Gd0.700       | 0.300 | 61.1     | 5.6      | 35.2            |
| O7Cu3Ba2Pr0.400Gd0.600       | 0.400 | 45.8     | 2.8+     | 25.7            |
| O7Cu3Ba2Pr0.500Gd0.500       | 0.500 | 28.6     | 0.0      | 15.2            |
| O7Cu3Ba2Pr0.600Gd0.400       | 0.600 | 17.2++   | 0.0      | 9.2             |
| O7Cu3Ba2Pr0.700Gd0.300       | 0.700 | 3.1++    | 0.0      | 1.7             |
| O7Cu3Ba2Pr0.800Gd0.200       | 0.800 | 0.0      | 0.0      | 0               |
| O7Cu3Ba2Pr0.900Gd0.100       | 0.900 | 0.0      | 0.0      | 0               |

* Nearest measured: Er at x=0.076, Nd at x=0.080
+ Nd at x=0.40 estimated by linear interpolation between measured (0.30, 5.6) and (0.50, 0.0) -> 2.8
++ Er at x=0.60,0.70 estimated from linear fit: 102.0 - 141.3*x

## Reasoning Summary

The Tc of each Gd-series composition was predicted by linearly interpolating between the measured Er and Nd Tc curves, weighted by Gd's position in ionic radius between Er and Nd (f=0.467 toward Nd). This reflects the known monotonic dependence of Pr suppression rate on RE ionic radius in REBa2Cu3O7 cuprates.
