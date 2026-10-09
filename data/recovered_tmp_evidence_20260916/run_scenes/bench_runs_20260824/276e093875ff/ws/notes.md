# Notes

## Scaffold Grouping (free)
Used `group_by_scaffold()` to identify 4 scaffold groups:

1. **Scaffold 1** `COc1ccc(cc1)C(=O)N` (m05's group): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **Scaffold 2** `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **Scaffold 3** `OCC(O)C(O)C(O)C(=O)N`: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **Scaffold 4** `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain_len=11, scaffold 1)

### Measured QED values for scaffold 1 members:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### QED component analysis (scaffold 1):
- **MW** score increases with chain: 0.33 -> 0.68 (MW approaching ideal range)
- **ALOGP** score increases then plateaus: 0.87 -> 0.997
- **ROTB** score decreases significantly: 0.99 -> 0.53 (main driver of QED decline)
- **ALERTS** drops at chain 4: 0.84 -> 0.64 (step change)
- **HBA, HBD, PSA, AROM**: constant across the series

### Polynomial fits for m05 (chain 11):
- 5-point degree-2 (chains 2-6): **0.5416** (RMSE 0.012)
- 3-point degree-2 (chains 4-6): **0.5257** (exact fit)
- 4-point degree-1 (chains 3-6): 0.6508
- 3-point degree-1 (chains 4-6): 0.7041

The two quadratic fits agree (~0.53-0.54), giving confidence in the prediction.
Linear fits overestimate because they don't capture the accelerating decline from ROTB/MW/ALOGP.

**Final prediction: 0.5416** (5-point degree-2 fit, most data-rich)

## Task 2: Find lead molecule (QED >= 0.6, PSA >= 40, not scaffold 1)

### PSA screening:
- Scaffold 2: PSA = 72.24 (>= 40)
- Scaffold 3: PSA = 110.02 (>= 40)
- Scaffold 4: PSA = 29.1 (< 40, eliminated)

### QED measurements for Task 2 candidates:
| mol_id | scaffold | chain | PSA     | QED    | Qualifies? |
|--------|----------|-------|---------|--------|------------|
| m09    | 2        | 2     | 72.24   | 0.5826 | No (QED < 0.6) |
| m02    | 2        | 3     | 72.24   | 0.6046 | **Yes** |
| m12    | 2        | 4     | 72.24   | 0.4709 | No (QED < 0.6) |
| m01    | 3        | 4     | 110.02  | 0.3187 | No (QED < 0.6) |

### Selected lead: **m02**
- QED = 0.6046 >= 0.6
- PSA = 72.24 >= 40
- Scaffold 2 (not scaffold 1)
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC

## Budget usage: 12/12
- 3 pages of list_pool (pages 1-3)
- 9 measurements: m15, m24, m17, m22, m28, m09, m02, m12, m01
