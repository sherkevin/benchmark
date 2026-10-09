# Notes

## Scaffold Grouping
4 scaffolds identified:
1. **COc1ccc(cc1)C(=O)N** (methoxybenzamide): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar-amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl-amide): PSA=29.1 for all — excluded from Task 2 (PSA < 40)

## Task 1: Predict m05 QED

m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, chain_len=11, alogp=4.956, MW=305.46

Measured QED for congeneric series members:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Trend: QED peaks at chain 3, then declines as chain gets longer (higher logP, more rotatable bonds).

Polynomial fits for chain_len=11:
- **Degree 2 (all 5 pts)**: QED = **0.5416** (RMSE=0.01188) ← chosen as most physically reasonable
- Degree 1 (chains 3-6): QED = 0.6508 (RMSE=0.00883)
- Degree 1 (all 5 pts): QED = 0.695 (RMSE=0.01308)
- Degree 3 (all 5 pts): QED = 2.1323 (overfitting, physically impossible)

The degree-2 fit is chosen because:
- Uses all 5 data points
- Captures the curvature (peak at chain 3, then decline)
- QED penalties compound at high logP (4.956) and high rotb (12), so a steeper decline is expected
- Degree 3 overfits with 4 parameters on 5 points

**Predicted QED for m05: 0.5416**

## Task 2: Lead Molecule Selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's scaffold (scaffold 1).

Scaffold 4 excluded (PSA = 29.1 < 40).

Measured candidates:
| mol_id | scaffold | QED    | PSA    | Qualifies? |
|--------|----------|--------|--------|------------|
| m09    | 2        | 0.5826 | 72.24  | No (QED < 0.6) |
| m02    | 2        | 0.6046 | 72.24  | **YES** ✓  |
| m12    | 2        | 0.4709 | 72.24  | No (QED < 0.6) |
| m32    | 2        | 0.4683 | 72.24  | No (QED < 0.6) |
| m01    | 3        | 0.3187 | 110.02 | No (QED < 0.6) |
| m33    | 2        | 0.4591 | 72.24  | No (QED < 0.6) |
| m29    | 2        | 0.4446 | 72.24  | No (QED < 0.6) |

**Lead molecule: m02** (SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`)
- Measured QED: 0.6046
- PSA: 72.24
- Not in m05's series (scaffold 2 vs scaffold 1)

## Budget Used
12/12 measurements used:
1. m15 → 0.7621
2. m24 → 0.7919
3. m28 → 0.7518
4. m17 → 0.7498
5. m22 → 0.7377
6. m09 → 0.5826
7. m02 → 0.6046
8. m12 → 0.4709
9. m32 → 0.4683
10. m01 → 0.3187
11. m33 → 0.4591
12. m29 → 0.4446
