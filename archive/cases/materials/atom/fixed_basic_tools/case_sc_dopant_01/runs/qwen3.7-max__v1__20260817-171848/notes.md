# Notes: Predicting Tc for Gd(1-x)Pr(x)Ba2Cu3O7

## Measured Data

### Er family (RE = Erbium, Z=68)
| x (Pr) | Tc (K) |
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

### Nd family (RE = Neodymium, Z=60)
| x (Pr) | Tc (K) |
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

## Key Observations

1. **Er family** has a gradual Tc suppression: Tc drops roughly linearly from ~94 K at x=0.03 to ~29 K at x=0.5, reaching 0 around x≈0.7–0.8.
2. **Nd family** has a much steeper Tc suppression: Tc drops rapidly from ~85 K at x=0.03 to ~6 K at x=0.3, reaching 0 by x≈0.4–0.5.
3. Nd's Tc suppression rate (≈294 K per unit x) is about 2× Er's (≈139 K per unit x).
4. The difference is physically explained by the ionic radius mismatch between RE and Pr: Nd (1.109 Å) is close in size to Pr (1.126 Å), so Pr substitutes more easily and causes more pair-breaking; Er (1.004 Å) is far from Pr, making substitution more disruptive to the lattice.

## Interpolation Strategy for Gd (Z=64)

Gd sits exactly midway between Nd (Z=60) and Er (Z=68) in atomic number, and nearly midway in ionic radius:
- Nd³⁺: 1.109 Å, Gd³⁺: 1.053 Å, Er³⁺: 1.004 Å (CN=8)
- Gd fraction from Nd to Er by ionic radius: (1.109–1.053)/(1.109–1.004) = 0.53 → essentially 50/50.

**Approach:** For each Gd x-value, I linearly interpolated Tc_Er(x) and Tc_Nd(x) from measured data, then averaged them: Tc_Gd(x) = (Tc_Er(x) + Tc_Nd(x)) / 2.

For Nd at x=0.4 (not measured): linearly interpolated between x=0.3 (5.6 K) and x=0.5 (0 K) → 2.8 K.
For Er at x=0.6, 0.7 (not measured): linearly interpolated between x=0.5 (28.6 K) and x=0.8 (0 K) → 19.1 K and 9.5 K respectively.

## Predictions

| Composition | x | Tc_Er(x) | Tc_Nd(x) | Tc_Gd(x) |
|---|---|---|---|---|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 93.9 | 85.1 | 89.5 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 90.9 | 68.9 | 79.9 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 82.9 | 40.7 | 61.8 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 76.4 | 31.4 | 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 61.1 | 5.6 | 33.4 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 45.8 | 2.8 | 24.3 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 28.6 | 0.0 | 14.3 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 19.1 | 0.0 | 9.5 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 9.5 | 0.0 | 4.8 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 | 0.0 | 0.0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 | 0.0 | 0.0 |

The Gd curve lies between the Nd and Er curves at all x values, with a smooth monotonic decrease in Tc from ~90 K to 0 K.
