# Notes

## Scaffold Grouping (free tool)
4 groups identified:
- **Group 1** (COc1ccc(cc1)C(=O)N): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11) — m05's series
- **Group 2** (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **Group 3** (OCC(O)C(O)C(O)C(=O)N): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **Group 4** (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain_len=11)

Measured all 5 synthesized members of m05's series:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fitted degree-2 polynomial through all 5 points:
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE on training points: 0.01188
- **Prediction at chain_len=11: QED = 0.5416**

Cross-checks:
- Linear fit (all 5 pts): 0.695
- Linear fit (pts 4,5,6 only): 0.7041
- The quadratic fit captures the peak at chain_len~3 and accelerating decline, which is physically reasonable (long chains → high MW, high logP → lower drug-likeness).

## Task 2: Find lead molecule

Constraints: QED ≥ 0.6, psa ≥ 40, not in m05's series.

PSA by group (same for all members in a congeneric series):
- Group 1: 38.33 (same series as m05, excluded)
- Group 2: 72.24 ✓
- Group 3: 110.02 ✓
- Group 4: 29.1 ✗ (below 40)

Candidates must come from Groups 2 or 3. Measured Group 2 members with short chains (better drug-likeness):
| mol_id | chain_len | QED    | psa    | Qualifies? |
|--------|-----------|--------|--------|------------|
| m09    | 2         | 0.5826 | 72.24  | No (QED < 0.6) |
| m02    | 3         | 0.6046 | 72.24  | **Yes** ✓  |
| m12    | 4         | 0.4709 | 72.24  | No (QED < 0.6) |

**Selected lead: m02** (QED=0.6046, psa=72.24, Group 2 scaffold ≠ m05's scaffold)

## Budget used
- list_pool: 3 pages (pages 1-3)
- measure_qed: 8 measurements (m15, m24, m17, m22, m28, m09, m02, m12)
- Total: 11 of 12 budget units used
