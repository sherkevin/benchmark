# Notes

## Scaffold Analysis

Used `group_by_scaffold()` to identify 4 congeneric series:

1. **COc1ccc(cc1)C(=O)N** (m05's scaffold): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05

m05 (chain_len=11) has never been synthesized. Measured all 5 available congeneric series members:

| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fitted degree-2 polynomial on all 5 points → prediction at chain_len=11: **QED = 0.5416** (RMSE = 0.01188).

Cross-checked with other fits:
- Linear on all 5: 0.695 (RMSE 0.013)
- Linear on last 4: 0.651 (RMSE 0.009)
- Degree 2 on last 4: 0.938 (unphysical — upward parabola)

The degree-2 fit on all 5 points is the best model: it captures the slight curvature (QED peaks at chain_len=3 then declines) and extrapolation to the long chain (alogP=4.956) giving a physically reasonable QED ~0.54.

## Task 2: Find a lead molecule

Constraints: QED ≥ 0.6, psa ≥ 40, not in m05's scaffold.

Scaffold 4 (biphenyl) excluded: psa = 29.1 < 40.
Candidates from scaffolds 2 and 3:

| mol_id | scaffold | chain_len | psa     | QED    | Pass? |
|--------|----------|-----------|---------|--------|-------|
| m09    | 2        | 2         | 72.24   | 0.5826 | No (QED < 0.6) |
| m02    | 2        | 3         | 72.24   | 0.6046 | **Yes** |
| m12    | 2        | 4         | 72.24   | 0.4709 | No |
| m32    | 2        | 5         | 72.24   | 0.4683 | No |
| m33    | 2        | 6         | 72.24   | 0.4591 | No |
| m01    | 3        | 4         | 110.02  | 0.3187 | No |
| m03    | 3        | 6         | 110.02  | 0.3282 | No |

**Lead: m02** — QED = 0.6046 ≥ 0.6, psa = 72.24 ≥ 40, different scaffold from m05. ✓

## Measurement Budget

Used 12/12 measurements: m15, m24, m28, m17, m22, m02, m12, m32, m09, m33, m01, m03.
