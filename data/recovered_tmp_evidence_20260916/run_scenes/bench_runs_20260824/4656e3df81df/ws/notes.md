# Notes

## Scaffold Grouping
4 scaffolds identified:
1. `COc1ccc(cc1)C(=O)N` — m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)** ← target
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Measurements (12/12 used)

### Scaffold 1 (m05's series):
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Scaffold 2 candidates:
| mol_id | chain | QED    | PSA    |
|--------|-------|--------|--------|
| m09    | 2     | 0.5826 | 72.24  |
| m02    | 3     | 0.6046 | 72.24  |
| m12    | 4     | 0.4709 | 72.24  |
| m32    | 5     | 0.4683 | 72.24  |
| m33    | 6     | 0.4591 | 72.24  |

### Scaffold 3 candidates:
| mol_id | chain | QED    | PSA    |
|--------|-------|--------|--------|
| m14    | 2     | 0.3237 | 110.02 |
| m01    | 4     | 0.3187 | 110.02 |

## Task 1: Predicting m05 QED

Fitted degree-2 polynomial on all 5 scaffold-1 data points:
- xs = [2, 3, 4, 5, 6], ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Predicted QED at chain_len=11: **0.5416**
- RMSE on fit: 0.0119

Physical reasoning: m05 has MW=305, ALOGP=4.96, ROTB=12 — all significantly worse than m22 (chain 6). QED components for m22 show ROTB score already at 0.53; at chain 11 this would be much worse. The downward quadratic extrapolation is physically reasonable.

## Task 2: Lead Selection

Constraints: QED >= 0.6, PSA >= 40, not in m05's series.

- Scaffold 4 has PSA=29.1 — eliminated.
- Scaffold 3 has PSA=110.02 but measured QEDs are ~0.32 — too low.
- Scaffold 2 has PSA=72.24 and m02 (chain 3) has QED=0.6046 >= 0.6.

**Selected: m02** with measured QED = 0.6046, PSA = 72.24, scaffold 2.
