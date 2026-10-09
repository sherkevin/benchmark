# Notes

## Scaffold Groups (from group_by_scaffold)
4 scaffold groups identified:
1. `COc1ccc(cc1)C(=O)N` — m05's group: m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain_len=11, scaffold 1)

### Measured congeners of m05:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend fitting for m05 (chain_len=11):
- **Quadratic (degree=2), all 5 points**: QED = 0.5416 (RMSE=0.0119) — SELECTED
- Linear (degree=1), all 5 points: QED = 0.695 (RMSE=0.0131)
- Linear (degree=1), chains 3-6 only: QED = 0.6508 (RMSE=0.0088)
- Quadratic, chains 3-6 only: QED = 0.9378 (unphysical — positive curvature)

Chose quadratic on all 5 points = 0.5416 as it uses all data, default degree=2, and negative curvature is physically reasonable (m05 has alogp=4.956 and rotb=12).

## Task 2: Pick molecule with QED >= 0.6, PSA >= 40, not in scaffold 1

### Measured QED for Task 2 candidates:
| mol_id | scaffold | PSA     | QED    | Pass? |
|--------|----------|---------|--------|-------|
| m09    | 2        | 72.24   | 0.5826 | No (QED < 0.6) |
| m02    | 2        | 72.24   | 0.6046 | **Yes** |
| m12    | 2        | 72.24   | 0.4709 | No (QED < 0.6) |

### Selected: m02
- QED = 0.6046 >= 0.6 ✓
- PSA = 72.24 >= 40 ✓
- Scaffold 2 ≠ Scaffold 1 (m05's) ✓

## Budget: 13 of 17 units used (8 measurements + 5 pages)
