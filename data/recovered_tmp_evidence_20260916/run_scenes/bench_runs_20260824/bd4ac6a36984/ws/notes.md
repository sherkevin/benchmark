# Notes

## Scaffold Groups (from group_by_scaffold, free)
- **Group 1** (m05's scaffold `COc1ccc(cc1)C(=O)N`): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
- **Group 2** (`O=[N+]([O-])c1ccc(cc1)C(=O)N`): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **Group 3** (`OCC(O)C(O)C(O)C(=O)N`): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **Group 4** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## PSA by scaffold (constant within each group)
- Group 1: 38.33 (< 40, ineligible for Task 2)
- Group 2: 72.24 (>= 40)
- Group 3: 110.02 (>= 40)
- Group 4: 29.1 (< 40, ineligible for Task 2)

## Budget usage (12 total)
- Pages listed: 4 (pages 1-4, covering m01-m16) = 4 budget
- Measurements: 8 = 8 budget
- Total: 12/12

## Task 1: Predict m05 QED

### Group 1 QED measurements
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |
| **m05**| **11**    | **?**  |

### Trend fits for m05 (chain_len=11)
- Quadratic (all 5 pts): **0.5416** (RMSE=0.0119) -- chosen as best model
- Linear (all 5 pts): 0.695 (RMSE=0.0131)
- Linear (chains 3-6): 0.6508 (RMSE=0.0088)

Chose quadratic with all 5 points because:
1. Captures non-linear rise-then-fall pattern
2. m05's alogp=4.956 and rotb=12 impose significant QED penalties
3. Extrapolation to chain=11 should show accelerating QED decline

**Predicted m05 QED: 0.5416**

## Task 2: Lead selection

### Candidates measured
| mol_id | group | psa     | QED    | QED>=0.6? | psa>=40? | not Group1? | Pass? |
|--------|-------|---------|--------|-----------|----------|-------------|-------|
| m09    | G2    | 72.24   | 0.5826 | No        | Yes      | Yes         | No    |
| m14    | G3    | 110.02  | 0.3237 | No        | Yes      | Yes         | No    |
| m02    | G2    | 72.24   | 0.6046 | Yes       | Yes      | Yes         | **Yes** |

**Lead: m02** (Group 2, chain_len=3, QED=0.6046, psa=72.24)
- Measured QED >= 0.6: Yes
- PSA >= 40: Yes
- Not in same congeneric series as m05: Yes
