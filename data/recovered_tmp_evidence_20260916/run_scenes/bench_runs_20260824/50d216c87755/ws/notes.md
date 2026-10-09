# Notes

## Task Overview
- 33 molecules in pool, 4 scaffold groups
- Budget: 12 QED measurements total
- Task 1: Predict QED of m05 (unsynthesized, chain_len=11)
- Task 2: Find a molecule with QED≥0.6, PSA≥40, not in m05's series

## Scaffold Groups

### Group A (m05's series): COc1ccc(cc1)C(=O)N — methoxybenzamides
- m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6), m05 (chain 11, unsynthesized)

### Group B: O=[N+]([O-])c1ccc(cc1)C(=O)N — nitrobenzamides (PSA=72.24)
- m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)

### Group C: OCC(O)C(O)C(O)C(=O)N — sugar amides (PSA=110.02)
- m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)

### Group D: c1ccc(cc1)-c1ccc(cc1)C(=O)N — biphenyl amides (PSA=29.1)
- All have PSA < 40, so ineligible for Task 2

## Measurements (12 total)

### m05's series (5 measurements):
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Other series candidates (7 measurements):
| mol_id | series | chain_len | QED    | PSA    |
|--------|--------|-----------|--------|--------|
| m09    | B      | 2         | 0.5826 | 72.24  |
| m02    | B      | 3         | 0.6046 | 72.24  |
| m12    | B      | 4         | 0.4709 | 72.24  |
| m32    | B      | 5         | 0.4683 | 72.24  |
| m01    | C      | 4         | 0.3187 | 110.02 |
| m03    | C      | 6         | 0.3282 | 110.02 |
| m14    | C      | 2         | 0.3237 | 110.02 |

## Task 1: Predicting m05 QED

Used degree-2 polynomial fit on all 5 series members:
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE on training points: 0.01188
- Prediction at chain_len=11: **0.5416**

Cross-checks:
- Linear fit on all 5 points → 0.695 (doesn't capture curvature)
- Linear fit on chains 3-6 → 0.651 (ignores chain-2 point)
- Quadratic on chains 3-6 → 0.938 (overfits, curves back up unreasonably)

The quadratic on all 5 points is the best model because:
1. Captures the slight peak near chain 3 then decline
2. Physically reasonable: long chains increase ALOGP (m05=4.956) and ROTB (m05=12), reducing QED
3. QED components confirm: m22 (chain 6) already shows ROTB=0.5332 and ALERTS=0.6397; m05 would be worse

**Predicted QED for m05: 0.5416**

## Task 2: Lead Molecule Selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series.

Candidates evaluated:
- Series C (sugar amides): All QED < 0.35, too low
- Series B (nitrobenzamides): m02 (QED=0.6046) passes all constraints!
  - QED = 0.6046 ≥ 0.6 ✓
  - PSA = 72.24 ≥ 40 ✓
  - Not in m05's series (different scaffold) ✓

**Lead molecule: m02, measured QED = 0.6046**
