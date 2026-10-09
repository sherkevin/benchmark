# Reasoning Notes — Pr-doped RE-Ba-Cu-O Tc Prediction

## 1. Measured Data

### Er family: Pr_x Er_{1-x} Ba2 Cu3 O7

| x (Pr)  | Tc (K) |
|---------|--------|
| 0.030   | 93.9   |
| 0.076   | 91.1   |
| 0.150   | 82.9   |
| 0.200   | 76.4   |
| 0.300   | 61.1   |
| 0.400   | 45.8   |
| 0.500   | 28.6   |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

**Pattern**: Er family shows gradual Tc suppression. Tc decreases roughly linearly at ~14 K per 0.1 Pr, reaching zero between x=0.5 and x=0.8 (estimated ~x=0.65).

### Nd family: Pr_x Nd_{1-x} Ba2 Cu3 O7

| x (Pr)  | Tc (K) |
|---------|--------|
| 0.030   | 85.1   |
| 0.080   | 68.2   |
| 0.150   | 40.7   |
| 0.200   | 31.4   |
| 0.300   | 5.6    |
| 0.500   | 0.0    |
| 0.600   | 0.0    |
| 0.700   | 0.0    |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

**Pattern**: Nd family shows steep Tc suppression. Tc drops ~28 K per 0.1 Pr initially, reaching zero between x=0.3 and x=0.5 (estimated ~x=0.35).

## 2. Key Observation

Pr suppresses Tc much faster in the Nd family than in the Er family. This is consistent with the known physics of Pr-doped cuprates: the suppression mechanism (likely magnetic pair-breaking and/or hybridization of Pr 4f states with CuO2 planes) is stronger for larger rare-earth ions.

## 3. Interpolation Strategy for Gd Family

Gd³⁺ ionic radius ≈ 1.053 Å (VIII coordination), sitting between:
- Nd³⁺ ≈ 1.109 Å
- Er³⁺ ≈ 1.004 Å

Interpolation fraction (from Er toward Nd):
f = (r_Gd - r_Er) / (r_Nd - r_Er) = (1.053 - 1.004) / (1.109 - 1.004) = 0.049 / 0.105 ≈ 0.467

So: **Tc_Gd(x) ≈ 0.533 × Tc_Er(x) + 0.467 × Tc_Nd(x)**

Gd is nearly midway between Nd and Er, slightly closer to Er.

## 4. Predicted Gd Tc Values

| x (Pr)  | Tc_Er(x) | Tc_Nd(x) | Tc_Gd (interpolated) |
|---------|----------|----------|----------------------|
| 0.030   | 93.9     | 85.1     | 89.8                 |
| 0.078   | ~91.0    | ~68.9    | 80.7                 |
| 0.150   | 82.9     | 40.7     | 63.2                 |
| 0.200   | 76.4     | 31.4     | 55.4                 |
| 0.300   | 61.1     | 5.6      | 35.2                 |
| 0.400   | 45.8     | 0        | 24.4                 |
| 0.500   | 28.6     | 0        | 15.2                 |
| 0.600   | ~14.3    | 0        | 7.6                  |
| 0.700   | 0        | 0        | 0                    |
| 0.800   | 0        | 0        | 0                    |
| 0.900   | 0        | 0        | 0                    |

Interpolated Er values: x=0.078 → linear interp between (0.076, 91.1) and (0.150, 82.9) ≈ 90.9; x=0.600 → linear interp between (0.500, 28.6) and (0.800, 0) ≈ 19.1.

Tc goes to zero around x ≈ 0.60–0.65 for Gd family (intermediate between Er's ~0.65 and Nd's ~0.35).
