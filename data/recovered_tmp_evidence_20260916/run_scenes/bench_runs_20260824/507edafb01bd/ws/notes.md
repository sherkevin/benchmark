# Notes

## Task Overview
- 33 molecules in pool, 4 scaffold groups
- Budget: 12 units (shared between list_pool and measure_qed)
- m05 is in Group 1 (scaffold `COc1ccc(cc1)C(=O)N`), chain_len=11, never synthesized

## Scaffold Groups (from group_by_scaffold)
1. **Group 1** (COc1ccc(cc1)C(=O)N): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **Group 2** (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **Group 3** (OCC(O)C(O)C(O)C(=O)N): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **Group 4** (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05

### Measurements (Group 1 congeneric series)
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### QED Components Analysis
| Component | chain=2 | chain=3 | chain=4 | chain=5 | chain=6 |
|-----------|---------|---------|---------|---------|---------|
| MW        | 0.3319  | 0.4062  | 0.4914  | 0.5853  | 0.6836  |
| ALOGP     | 0.8736  | 0.9363  | 0.9768  | 0.9973  | 0.9973  |
| HBA       | 0.9305  | 0.9305  | 0.9305  | 0.9305  | 0.9305  |
| HBD       | 0.9866  | 0.9866  | 0.9866  | 0.9866  | 0.9866  |
| PSA       | 0.933   | 0.933   | 0.933   | 0.933   | 0.933   |
| ROTB      | 0.9927  | 0.9697  | 0.8476  | 0.6885  | 0.5332  |
| AROM      | 0.8273  | 0.8273  | 0.8273  | 0.8273  | 0.8273  |
| ALERTS    | 0.8425  | 0.8425  | 0.6397  | 0.6397  | 0.6397  |

Key trends: MW improves, ALOGP plateaus, ROTB drops sharply (biggest driver of QED decline), ALERTS drops at chain 4.

### Prediction via fit_trend
- **5-point degree-2: 0.5416** (RMSE=0.012) — chosen
- 5-point degree-1: 0.695 (RMSE=0.013)
- 4-point (3-6) degree-1: 0.6508
- 4-point (2,4,5,6) degree-2: 0.6698
- 5-point degree-3: 2.13 (unrealistic, overfitting)

Physical reasoning supports quadratic: ROTB desirability crashes for rotb=12, ALOGP starts declining at alogp=4.956.

**Prediction: target_qed = 0.5416**

## Task 2: Find lead molecule

### Constraints
1. QED >= 0.6
2. PSA >= 40
3. Not in Group 1 (m05's series)

### PSA by group
- Group 2: PSA = 72.24 (pass)
- Group 3: PSA = 110.02 (pass)
- Group 4: PSA = 29.1 (fail)

### Measurements for Task 2
| mol_id | group | QED    | PSA   | Pass? |
|--------|-------|--------|-------|-------|
| m09    | 2     | 0.5826 | 72.24 | No (QED < 0.6) |
| m02    | 2     | 0.6046 | 72.24 | **Yes** |

### Selected lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED measured: 0.6046
- PSA: 72.24
- Group 2, not same series as m05
- All three constraints satisfied

## Budget Usage
- 7 measurements: m15, m24, m28, m17, m22, m09, m02 = 7 units
- 3 page listings: pages 1-3 = 3 units
- Total: 10/12 units used
