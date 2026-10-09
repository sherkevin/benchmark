# Notes

## Scaffold Groups (from group_by_scaffold)
- **Group 1** (COc1ccc(cc1)C(=O)N, psa=38.33): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
- **Group 2** (O=[N+]([O-])c1ccc(cc1)C(=O)N, psa=72.24): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **Group 3** (OCC(O)C(O)C(O)C(=O)N, psa=110.02): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **Group 4** (c1ccc(cc1)-c1ccc(cc1)C(=O)N, psa=29.1): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Budget Usage
- Pages read: 5 (pages 1-5, 20 molecules seen)
- QED measurements: 7 (m15, m28, m22, m17, m09, m02, m12)
- Total budget used: 12/12

## Task 1: Predict m05 QED

m05 is in Group 1 with chain_len=11. Measured 4 members of its congeneric series:

| mol_id | chain_len | QED (measured) |
|--------|-----------|-----------------|
| m15    | 2         | 0.7621          |
| m28    | 4         | 0.7518          |
| m17    | 5         | 0.7498          |
| m22    | 6         | 0.7377          |

Trend: QED decreases with increasing chain length.

**Quadratic fit** (degree=2) using all 4 points:
- RMSE: 0.00174
- **Prediction at chain_len=11: QED = 0.6698 ~ 0.67**

Cross-checks:
- Linear fit: 0.7123 (doesn't capture curvature)
- Cubic fit: 0.2054 (overfitting)
- 3-point quadratic (chains 4,5,6): 0.5257 (overfitting noise)

## Task 2: Find a lead molecule

Constraints: QED >= 0.6, psa >= 40, not in Group 1.

PSA filter: only Groups 2 (psa=72.24) and 3 (psa=110.02) qualify.

Measured Group 2 candidates:
| mol_id | QED    | Qualifies? |
|--------|--------|------------|
| m09    | 0.5826 | No         |
| m02    | 0.6046 | **YES**    |
| m12    | 0.4709 | No         |

**Selected lead: m02** (QED=0.6046, psa=72.24, different scaffold from m05)
