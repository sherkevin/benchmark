# Notes

## Task Overview
- 33 molecules, 4 scaffold groups, shared budget of 17 units for list_pool + measure_qed.
- Task 1: Predict QED of m05 (never synthesized) from its congeneric series trend.
- Task 2: Find one pool molecule with QED >= 0.6, PSA >= 40, and not in m05's series.

## Scaffold Groups (from group_by_scaffold, free)
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11) — PSA=38.33
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) — PSA=72.24
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) — PSA=110.02
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) — PSA=29.10

## Task 2 Constraint Analysis
- Scaffold 4: PSA=29.10 < 40 -> all eliminated
- Scaffold 1 (m05's series): PSA=38.33 < 40 AND excluded as same series -> eliminated
- Scaffold 2: PSA=72.24 >= 40 -> candidates
- Scaffold 3: PSA=110.02 >= 40 -> candidates

## Measurements (17 budget used)
### Pages listed: 1-4 (4 units) -> saw m01-m16 descriptors

### m05 series QED measurements (5 units):
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Task 2 candidate measurements (8 units):
| mol_id | scaffold | chain | QED    | Qualifies? |
|--------|----------|-------|--------|------------|
| m09    | 2        | 2     | 0.5826 | No (QED < 0.6) |
| m02    | 2        | 3     | 0.6046 | **YES** |
| m12    | 2        | 4     | 0.4709 | No (QED < 0.6) |
| m32    | 2        | 5     | 0.4683 | No (QED < 0.6) |
| m14    | 3        | 2     | 0.3237 | No (QED < 0.6) |
| m33    | 2        | 6     | 0.4591 | No (QED < 0.6) |
| m01    | 3        | 4     | 0.3187 | No (QED < 0.6) |
| m27    | 3        | 3     | 0.3330 | No (QED < 0.6) |

## Task 1: m05 QED Prediction

### Trend fits:
- **Quadratic, all 5 points (chains 2-6): QED = 0.5416** (RMSE = 0.01188) <- selected
- Linear, all 5 points: QED = 0.695 (RMSE = 0.01308)
- Linear, chains 3-6 only: QED = 0.6508 (RMSE = 0.00883)
- Quadratic, chains 3-6: QED = 0.9378 (unphysical, curves upward)

### Justification for quadratic on all 5 points:
- Uses all available data from the congeneric series
- Physically justified: as chain lengthens, ALOGP (4.956) and ROTB (12) both enter heavily penalized regions
- QED component analysis confirms: ROTB desirability drops from 0.99 (chain 2) to 0.53 (chain 6), will drop further at chain 11
- RMSE of 0.012 is good

### QED Components (from qed_components, free):
- m15 (chain 2): MW=0.33, ALOGP=0.87, ROTB=0.99, ALERTS=0.84
- m22 (chain 6): MW=0.68, ALOGP=1.00, ROTB=0.53, ALERTS=0.64
- m02: MW=0.50, ALOGP=0.92, ROTB=0.97, PSA=0.93, ALERTS=0.24

## Final Answer
- Task 1 (m05 QED): 0.54 (from quadratic fit on all 5 congeneric series members)
- Task 2 (lead molecule): m02, measured QED = 0.6046 (PSA=72.24, scaffold 2, chain 3)
