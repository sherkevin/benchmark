# Notes

## Task 1: Predict QED for m05

m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has chain_len=11 and belongs to the scaffold `COc1ccc(cc1)C(=O)N`.

The congeneric series for this scaffold includes:
- m15 (chain_len=2)
- m24 (chain_len=3)
- m28 (chain_len=4)
- m17 (chain_len=5)
- m22 (chain_len=6)
- m05 (chain_len=11)

Measured QED values for the series:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Used `fit_trend` with degree=2 polynomial to predict QED at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Find a lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in the same congeneric series as m05 (i.e., NOT scaffold `COc1ccc(cc1)C(=O)N`)

Measured QED for candidates with psa ≥ 40 from other scaffolds:
| mol_id | scaffold | psa | QED | Meets criteria? |
|--------|----------|-----|-----|-----------------|
| m01    | OCC(O)C(O)C(O)C(=O)N | 110.02 | 0.3187 | No (QED < 0.6) |
| m02    | O=[N+]([O-])c1ccc(cc1)C(=O)N | 72.24 | 0.6046 | **Yes** |
| m14    | OCC(O)C(O)C(O)C(=O)N | 110.02 | 0.3237 | No (QED < 0.6) |
| m09    | O=[N+]([O-])c1ccc(cc1)C(=O)N | 72.24 | 0.5826 | No (QED < 0.6) |

**Selected lead molecule: m02**
- QED measured: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold ✓)

## Budget used
Total measurements: 9 out of 12
