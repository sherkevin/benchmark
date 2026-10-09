# Notes

## Overview
- 33 molecules in 4 congeneric series (scaffolds)
- Budget: 17 units total (9 for listing pages, 8 for measurements)

## Scaffold Groups (from group_by_scaffold)
1. `COc1ccc(cc1)C(=O)N` (anisole amide): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro benzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` (sugar-like): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain 11, scaffold 1)

m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC, chain_len=11, MW=305.46, ALOGP=4.956, PSA=38.33, ROTB=12, AROM=1

### Measured QEDs in m05's congeneric series:
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Polynomial fits predicting QED at chain 11:
- Linear (5 pts): 0.695 (RMSE=0.013) — likely too high, ignores accelerating penalties
- Quadratic (5 pts): 0.542 (RMSE=0.012) — chosen as best estimate
- Quadratic (last 3 pts, chains 4-6): 0.526 — consistent with full fit
- Cubic (5 pts): 2.13 — nonsensical (overfit, QED bounded [0,1])

### QED component analysis (from qed_components):
As chain length increases from 2 to 6:
- MW desirability: increases (0.33 → 0.68) — helps QED
- ALOGP desirability: increases then saturates (~1.0) — but m05 has ALOGP=4.956, well past the optimum, so this will drop sharply
- ROTB desirability: drops steeply (0.99 → 0.53) — at chain 11 (ROTB=12), will be very low (~0.02-0.05)
- ALERTS desirability: drops from 0.84 to 0.64 then stabilizes
- HBA, HBD, PSA, AROM: constant across the series

The accelerating penalties from ALOGP overshoot (4.96 vs ~3.0 at chain 6) and ROTB (12 vs 7) support the downward-curving quadratic extrapolation over the linear one.

**Final prediction for m05 QED: 0.54**

## Task 2: Find molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's series

### PSA screening across scaffolds:
- Scaffold 4 (biphenyl): PSA = 29.1 for all members — ALL FAIL PSA constraint
- Scaffold 1 (anisole): PSA = 38.33 for all members — ALL FAIL PSA constraint (and m05's series anyway)
- Scaffold 2 (nitro): PSA = 72.24 for all members — ALL PASS ✓
- Scaffold 3 (sugar): PSA = 110.02 for all members — ALL PASS ✓

### Measured candidates from scaffold 2 (shortest chains, best drug-like properties):
| mol_id | chain | QED    | PSA    | Pass all? |
|--------|-------|--------|--------|-----------|
| m09    | 2     | 0.5826 | 72.24  | No (QED < 0.6) |
| m02    | 3     | 0.6046 | 72.24  | **Yes** ✓ |
| m12    | 4     | 0.4709 | 72.24  | No (QED < 0.6) |

### Result: **m02** satisfies all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Not in m05's congeneric series (scaffold 2 ≠ scaffold 1) ✓

m02 SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC, MW=208.22, ALOGP=1.735, PSA=72.24, ROTB=4, AROM=1
