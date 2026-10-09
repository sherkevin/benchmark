# Notes

## Scaffold Analysis (free tool)
4 scaffold groups identified:
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11) — psa=38.33
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) — psa=72.24
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) — psa=110.02
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) — psa=29.1

## Budget Usage
- Pages listed: 4 (pages 1-4, cost 4)
- QED measurements: 8 (cost 8)
- Total: 12/12

## Task 1: Predict QED of m05

m05 is in scaffold 1 with chain_len=11. Measured its congeneric series:

| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Trend: QED peaks around chain 3 then decreases. Fitting degree-2 polynomial through all 5 points:
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
- RMSE: 0.01188
- **Prediction at chain_len=11: QED = 0.5416**

Cross-checks:
- Linear fit (all 5 points): 0.695
- Linear fit (last 4 points): 0.6508
- Quadratic is most chemically reasonable — long alkyl chains increase logP (m05 alogP=4.956) and rotatable bonds (12), penalizing QED.

## Task 2: Find lead molecule (QED>=0.6, psa>=40, not m05's series)

PSA filter (must be >= 40):
- Scaffold 1 (psa=38.33): excluded (below 40 AND same series as m05)
- Scaffold 2 (psa=72.24): candidates
- Scaffold 3 (psa=110.02): candidates
- Scaffold 4 (psa=29.1): excluded (below 40)

Measured 3 candidates from scaffold 2:
- m12 (chain 4): QED=0.4709 — fails (below 0.6)
- m02 (chain 3): QED=0.6046 — **PASSES ALL CRITERIA**
- m09 (chain 2): QED=0.5826 — fails (below 0.6)

**Selected lead: m02**
- Measured QED: 0.6046 >= 0.6 ✓
- PSA: 72.24 >= 40 ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05) ✓

## Final Answers
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
