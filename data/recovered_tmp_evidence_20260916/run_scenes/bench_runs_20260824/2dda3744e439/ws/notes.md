# Notes

## Scaffold Grouping

Used `group_by_scaffold()` to identify 4 congeneric series:

1. **COc1ccc(cc1)C(=O)N** (methoxybenzamide): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar-acid): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Measurements (12/12 used)

### m05's congeneric series (Task 1):
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Other series (Task 2 candidates):
| mol_id | series  | QED    | PSA     |
|--------|---------|--------|---------|
| m09    | nitro   | 0.5826 | 72.24   |
| m02    | nitro   | 0.6046 | 72.24   |
| m12    | nitro   | 0.4709 | 72.24   |
| m32    | nitro   | 0.4683 | 72.24   |
| m33    | nitro   | 0.4591 | 72.24   |
| m01    | sugar   | 0.3187 | 110.02  |
| m03    | sugar   | 0.3282 | 110.02  |

## Task 1: Predicting m05's QED

Fitted polynomial through the 5 measured members of m05's series (chain lengths 2-6) and extrapolated to chain length 11.

- **Linear (degree 1)**: QED = 0.695, RMSE = 0.01308
- **Quadratic (degree 2)**: QED = 0.5416, RMSE = 0.01188 ← best fit
- **Cubic (degree 3)**: QED = 2.1323 (nonsense, overfitting)

The quadratic model is preferred: lowest RMSE among sensible models, and the prediction (0.54) is physically reasonable given m05's high logP (4.956) and long chain.

**Predicted QED for m05: 0.5416**

## Task 2: Lead Selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series.

PSA filter eliminates biphenyl series (PSA=29.1) and m05's own series (PSA=38.33).
Candidates from nitro (PSA=72.24) and sugar (PSA=110.02) series.

Only **m02** satisfies all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Nitro series ≠ m05's methoxybenzamide series ✓

**Lead: m02, measured QED = 0.6046**
