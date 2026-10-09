# Notes

## Task Overview
- 33 molecules in pool, 4 scaffold groups (congeneric series)
- Budget: 12 units (shared between list_pool and measure_qed)
- Budget used: 12/12 (3 pages listed + 9 measurements)

## Scaffold Groups
1. **COc1ccc(cc1)C(=O)N** — m05's series: m15(c2), m24(c3), m28(c4), m17(c5), m22(c6), m05(c11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** — m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10)
3. **OCC(O)C(O)C(O)C(=O)N** — m14(c2), m27(c3), m01(c4), m19(c5), m03(c6), m21(c7), m18(c8), m07(c9), m25(c10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** — m13(c2), m11(c3), m04(c4), m30(c5), m23(c6), m20(c7), m26(c8), m08(c9), m16(c10)

## Task 1: Predict QED of m05 (chain length 11, never synthesized)

### Measured QED values in m05's series:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### QED component trends:
- MW score: INCREASING (0.33 to 0.68) — MW moving toward sweet spot
- ALOGP score: increasing then plateau (0.87 to 0.997) — but m05 has alogp=4.956 (high)
- ROTB score: DECREASING rapidly (0.99 to 0.53) — m05 has rotb=12, will be very low
- HBA, HBD, PSA, AROM: constant across series
- ALERTS: drops from 0.8425 to 0.6397 at chain 4, then constant

### Polynomial fits for QED at chain 11:
- Degree 2, all 5 points: **0.5416** (RMSE=0.012) — chosen
- Degree 1, all 5 points: 0.695 (RMSE=0.013)
- Degree 1, chains 3-6: 0.6508 (RMSE=0.009)
- Degree 1, chains 4-6: 0.7041 (RMSE=0.002)
- Degree 2, chains 4-6: 0.5257

### Chosen prediction: 0.5416
Rationale: Quadratic fit on all 5 data points captures the non-linear decline driven by increasing ROTB penalty and eventually high ALOGP. m05 has MW=305.46, ALOGP=4.956, PSA=38.33, ROTB=12.

## Task 2: Find molecule with QED>=0.6, PSA>=40, not in m05's series

### Elimination logic:
- Series 4 has PSA=29.1 for all members — all eliminated (PSA < 40)
- Series 2 has PSA=72.24 for all members — candidates
- Series 3 has PSA=110.02 for all members — candidates

### Measurements for Task 2:
| mol_id | series | QED    | PSA    | Qualifies? |
|--------|--------|--------|--------|------------|
| m02    | 2      | 0.6046 | 72.24  | YES        |
| m09    | 2      | 0.5826 | 72.24  | No (QED<0.6) |
| m12    | 2      | 0.4709 | 72.24  | No (QED<0.6) |
| m32    | 2      | 0.4683 | 72.24  | No (QED<0.6) |

### Chosen lead: m02
- Measured QED: 0.6046 >= 0.6
- PSA: 72.24 >= 40
- Series: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's series)
