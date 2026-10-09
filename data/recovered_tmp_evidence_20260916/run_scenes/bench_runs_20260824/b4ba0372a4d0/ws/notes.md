# Notes

## Strategy
1. Used `group_by_scaffold()` (free) to identify 4 scaffold groups and m05's congeneric series.
2. Listed 4 pages of the catalogue (cost: 4 budget units) to get descriptor values (especially psa).
3. Measured QED of 5 members of m05's series + 2 Task 2 candidates (cost: 7 budget units).
4. Used `fit_trend()` (free) to predict m05's QED.

Total budget used: 11 of 12.

## Scaffold Groups
| Scaffold | Members | PSA |
|----------|---------|-----|
| 1: COc1ccc(cc1)C(=O)N (m05's) | m15(2), m24(3), m28(4), m17(5), m22(6), m05(11) | 38.33 |
| 2: O=[N+]([O-])c1ccc(cc1)C(=O)N | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) | 72.24 |
| 3: OCC(O)C(O)C(O)C(=O)N | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) | 110.02 |
| 4: c1ccc(cc1)-c1ccc(cc1)C(=O)N | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) | 29.1 |

## Task 1: m05 QED Prediction
m05 has never been synthesized (probe_unavailable), so I predicted its QED from its congeneric series.

Measured QEDs in m05's congeneric series (scaffold: COc1ccc(cc1)C(=O)N):
| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

Quadratic fit (degree=2, all 5 points):
- Coefficients: [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188
- **Predicted QED at chain_len=11 (m05): 0.5416**

Cross-checks (free fit_trend):
- Linear fit (5 pts): 0.6950
- Linear fit (pts 3-6): 0.6508
- Quadratic fit (pts 3-6): 0.9378 (positive curvature, chemically unreasonable for long chains)

The degree-2 polynomial with all 5 points is the most reliable prediction. m05 has alogp=4.956 (very lipophilic) and rotb=12 (many rotatable bonds), both of which QED penalizes heavily. The accelerating decline captured by the negative quadratic coefficient is consistent with QED's strong penalty for high logP.

## Task 2: Lead Molecule Selection
Constraints: QED >= 0.6, psa >= 40, not in m05's congeneric series.

Only scaffolds 2 (psa=72.24) and 3 (psa=110.02) have psa >= 40.

Measured candidates:
- **m02** (scaffold 2, chain 3, alogp=1.735): QED = 0.6046 >= 0.6 ✓, psa = 72.24 >= 40 ✓, not m05's series ✓ → **SELECTED**
- m09 (scaffold 2, chain 2, alogp=1.345): QED = 0.5826 < 0.6 ✗

## Final Answers
- target_qed (m05): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
