# Notes

## Scaffold Groups
1. **COc1ccc(cc1)C(=O)N** (psa=38.33): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (psa=72.24): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (psa=110.02): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (psa=29.1): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain_len=11, scaffold group 1)

m05 has never been synthesized → must predict from congeneric series trend.

### Measurements in Group 1 (COc1ccc(cc1)C(=O)N):
| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### QED Component Analysis:
| chain_len | MW | ALOGP | HBA | HBD | PSA | ROTB | AROM | ALERTS |
|-----------|-------|-------|-------|-------|-------|-------|-------|--------|
| 2 (m15) | 0.332 | 0.874 | 0.931 | 0.987 | 0.933 | 0.993 | 0.827 | 0.843 |
| 3 (m24) | 0.406 | 0.936 | 0.931 | 0.987 | 0.933 | 0.970 | 0.827 | 0.843 |
| 4 (m28) | 0.491 | 0.977 | 0.931 | 0.987 | 0.933 | 0.848 | 0.827 | 0.640 |
| 5 (m17) | 0.585 | 0.997 | 0.931 | 0.987 | 0.933 | 0.689 | 0.827 | 0.640 |
| 6 (m22) | 0.684 | 0.997 | 0.931 | 0.987 | 0.933 | 0.533 | 0.827 | 0.640 |

Key trends: MW desirability increases, ALOGP plateaus near 1.0, ROTB drops significantly, ALERTS drops at chain_len ≥ 4.

For m05 (chain_len=11): mw=305.46, alogp=4.956, rotb=12 → expect ROTB and ALOGP desirabilities to drop further.

### Polynomial Fits (QED vs chain_len):
- **Degree 2 (all 5 pts): QED(11) = 0.5416** (RMSE = 0.0119) ← selected
- Degree 1 (all 5 pts): QED(11) = 0.695 (RMSE = 0.0131)
- Degree 3 (all 5 pts): QED(11) = 2.13 (unphysical, curves upward)

Cross-validation: Group 2 at chain_len=10 has QED=0.37, showing similar long-chain decline pattern.

**Predicted QED for m05: 0.54**

## Task 2: Find lead molecule (QED ≥ 0.6, PSA ≥ 40, not in group 1)

PSA ≥ 40 eliminates group 1 (PSA=38.33) and group 4 (PSA=29.1).
Candidates: group 2 (PSA=72.24) and group 3 (PSA=110.02).

### Measurements:
| mol_id | group | chain_len | QED | PSA | QED≥0.6? |
|--------|-------|-----------|-------|------|----------|
| m09 | 2 | 2 | 0.5826 | 72.24 | ✗ |
| **m02** | **2** | **3** | **0.6046** | **72.24** | **✓** |
| m12 | 2 | 4 | 0.4709 | 72.24 | ✗ |
| m32 | 2 | 5 | 0.4683 | 72.24 | ✗ |
| m33 | 2 | 6 | 0.4591 | 72.24 | ✗ |
| m29 | 2 | 7 | 0.4446 | 72.24 | ✗ |
| m10 | 2 | 10 | 0.3724 | 72.24 | ✗ |

Group 3 not measured (very negative alogp values suggest low QED).

**Selected lead: m02** (QED=0.6046, PSA=72.24, scaffold O=[N+]([O-])c1ccc(cc1)C(=O)N ≠ m05's scaffold)

## Budget Used: 12/12 measurements
m15, m24, m28, m17, m22, m02, m12, m32, m09, m33, m29, m10
