# Notes

## Scaffold Grouping

4 scaffold groups identified:

1. **COc1ccc(cc1)C(=O)N** (m05's series, PSA=38.33): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (PSA=72.24): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (PSA=110.02): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (PSA=29.1): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05

Measured all 5 synthesizable members of m05's congeneric series:

| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

QED peaks at chain 3 then gradually decreases as chain length increases (consistent with rising logP).

Fitted degree-2 polynomial on all 5 points and extrapolated to chain 11: **QED = 0.5416** (RMSE = 0.01188)

This is consistent with m05's high alogp (4.956) which penalizes drug-likeness.

## Task 2: Find lead molecule (QED >= 0.6, PSA >= 40, not in m05's series)

PSA >= 40 rules out Groups 1 (38.33) and 4 (29.1). Candidates from Groups 2 and 3 only.

### Group 2 measurements (PSA = 72.24):
| mol_id | chain_len | QED    | Qualifies? |
|--------|-----------|--------|------------|
| m09    | 2         | 0.5826 | No (QED<0.6) |
| m02    | 3         | 0.6046 | **YES** |
| m12    | 4         | 0.4709 | No (QED<0.6) |
| m32    | 5         | 0.4683 | No (QED<0.6) |
| m33    | 6         | 0.4591 | No (QED<0.6) |

### Group 3 measurements (PSA = 110.02):
| mol_id | chain_len | QED    | Qualifies? |
|--------|-----------|--------|------------|
| m14    | 2         | 0.3237 | No (QED<0.6) |
| m01    | 4         | 0.3187 | No (QED<0.6) |

Group 3 molecules have very low QED due to high PSA and zero aromatic rings.

### Best lead: **m02**
- QED = 0.6046 >= 0.6 (check)
- PSA = 72.24 >= 40 (check)
- Not in m05's congeneric series (Group 2 vs Group 1) (check)

## Budget used: 12/12 measurements
