# Notes

# Notes

## Approach

### Scaffold Groups (from group_by_scaffold)
1. **COc1ccc(cc1)C(=O)N** (methoxyphenyl amide): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrophenyl amide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl amide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

### m05 belongs to Group 1 (scaffold COc1ccc(cc1)C(=O)N, chain length 11)

## Measurements (12 total used)

### Group 1 (m05's series) - 5 measurements:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Group 2 (Task 2 candidates, PSA=72.24) - 5 measurements:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m09    | 2         | 0.5826 |
| m02    | 3         | 0.6046 |
| m12    | 4         | 0.4709 |
| m32    | 5         | 0.4683 |
| m33    | 6         | 0.4591 |

### Group 3 (Task 2 candidates, PSA=110.02) - 2 measurements:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m14    | 2         | 0.3237 |
| m01    | 4         | 0.3187 |

## Task 1: Predict m05 QED

Fitted models for Group 1 (QED vs chain_len):
- **Quadratic (degree 2), all 5 pts**: QED(11) = **0.5416** (RMSE 0.01188)
  - Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
- Linear (degree 1), chains 3-6: QED(11) = 0.6508 (RMSE 0.00883)
- Linear (degree 1), all 5 pts: QED(11) = 0.6950 (RMSE 0.01308)
- Quadratic, chains 3-6: QED(11) = 0.9378 (unreasonable - upward curve artifact)

**Chosen prediction: 0.5416** (quadratic on all 5 data points, the default degree=2 model using all available congeneric data). This is physically reasonable given m05 has alogP=4.956 (near Lipinski limit), MW=305, and 12 rotatable bonds.

## Task 2: Lead Selection

Constraints: QED >= 0.6, PSA >= 40, not in m05's series (group 1).

- Group 4 (biphenyl): PSA = 29.1 -> fails PSA constraint
- Group 3 (sugar): m14 QED=0.32, m01 QED=0.32 -> fail QED constraint
- Group 2 (nitrophenyl): PSA = 72.24
  - m09: QED 0.5826 -> fails (< 0.6)
  - **m02: QED 0.6046 -> PASSES all constraints**
  - m12, m32, m33: QED < 0.5 -> fail

**Selected lead: m02** (QED=0.6046, PSA=72.24, different scaffold from m05)
