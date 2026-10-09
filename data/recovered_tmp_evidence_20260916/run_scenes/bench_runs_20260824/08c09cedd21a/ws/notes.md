# Notes: Predicting Tc of Gd-series RE(1-x)Pr(x)Ba2Cu3O7

## Database Discovery

Searched for all rare earth elements. Found 3 RE families in the database:
- **Gd** (11 compositions) — NOT synthesized, must predict
- **Nd** (10 compositions) — synthesized, measured
- **Er** (9 compositions) — synthesized, measured

## Measured Tc Data

### Nd series (Z=60, larger ionic radius)
| x(Pr) | Tc (K) |
|-------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600-0.900 | 0.0 |

### Er series (Z=68, smaller ionic radius)
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

## Pattern

- Pr suppresses Tc in all families
- Nd (larger RE): Tc drops steeply, reaches 0 by x~0.35-0.50
- Er (smaller RE): Tc drops gradually, still 28.6 K at x=0.50, reaches 0 by x~0.80
- Smaller RE ionic radius → slower Tc suppression by Pr

## Interpolation Strategy

Gd (Z=64) sits exactly at the midpoint between Nd (Z=60) and Er (Z=68).
Ionic radii (8-coord): Nd=1.109, Gd=1.053, Er=1.004 → fraction ≈ 0.53 ≈ 0.5

**Tc_Gd(x) = 0.5 × Tc_Nd(x) + 0.5 × Tc_Er(x)**

Using piecewise linear interpolation for continuous Nd and Er curves.

## Gd Predictions

| x(Pr) | Tc_Nd | Tc_Er | Tc_Gd |
|-------|-------|-------|-------|
| 0.030 | 85.1 | 93.9 | 89.5 |
| 0.078 | 68.9 | 90.9 | 79.9 |
| 0.150 | 40.7 | 82.9 | 61.8 |
| 0.200 | 31.4 | 76.4 | 53.9 |
| 0.300 | 5.6 | 61.1 | 33.4 |
| 0.400 | 2.8 | 45.8 | 24.3 |
| 0.500 | 0.0 | 28.6 | 14.3 |
| 0.600 | 0.0 | 19.1 | 9.5 |
| 0.700 | 0.0 | 9.5 | 4.8 |
| 0.800 | 0.0 | 0.0 | 0 |
| 0.900 | 0.0 | 0.0 | 0 |
