# Notes

## Task Overview
- Predict QED of m05 (chain_len=11, scaffold COc1ccc(cc1)C(=O)N) from its congeneric series
- Pick one molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's series

## Scaffold Groups (from group_by_scaffold)
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## PSA Screening for Task 2
- Scaffold 2: PSA = 72.24 ≥ 40 ✓
- Scaffold 3: PSA = 110.02 ≥ 40 ✓
- Scaffold 4: PSA = 29.1 < 40 ✗ (eliminated)

## Measurements (12 total)

### Scaffold 1 (m05's series) — for Task 1
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Scaffold 2 — candidates for Task 2
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m09    | 2         | 0.5826 |
| m02    | 3         | 0.6046 |
| m12    | 4         | 0.4709 |
| m32    | 5         | 0.4683 |
| m33    | 6         | 0.4591 |
| m29    | 7         | 0.4446 |

### Scaffold 3 — checked one
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m14    | 2         | 0.3237 |

## Task 1: Predicting m05 QED

### Quadratic fit (degree=2) on all 5 scaffold-1 points
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE = 0.01188
- Prediction at chain=11: **0.5416**

### Cross-checks
- Linear fit on chains 3-6: prediction = 0.6508
- Linear fit on chains 4-6: prediction = 0.7041
- The quadratic is preferred because it captures the curvature (peak at chain ~3 then decline) and uses all 5 data points.
- Physical rationale: m05 has alogp=4.956 (near Lipinski limit), MW=305.46. The long C11 chain makes it very lipophilic, which should significantly penalize QED. The quadratic decline to ~0.54 is chemically reasonable.

### Final Task 1 answer: **0.5416**

## Task 2: Lead Selection

Candidates must have: QED ≥ 0.6, PSA ≥ 40, not in scaffold 1.

- m02: QED = 0.6046 ≥ 0.6 ✓, PSA = 72.24 ≥ 40 ✓, scaffold 2 ✓ → **QUALIFIES**
- m09: QED = 0.5826 < 0.6 ✗
- All other measured scaffold 2 molecules: QED < 0.6
- m14 (scaffold 3): QED = 0.3237 < 0.6 ✗ (and scaffold 3 molecules have very high PSA and negative logP, unlikely to have high QED)

### Final Task 2 answer: **m02** with measured QED = **0.6046**
