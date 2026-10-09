# Notes

## Task 1: Predict QED for m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so I cannot measure it directly. I need to predict its QED from its congeneric series.

### Congeneric series for m05
From `group_by_scaffold()`, m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain length 11.
Other members: m15 (len=2), m24 (len=3), m28 (len=4), m17 (len=5), m22 (len=6).

### Measurements taken for m05 series
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Polynomial fit
Used `fit_trend` with degree=2 on chain lengths [2,3,4,5,6] and QEDs above.
Prediction for x_query=11: **0.5416**

## Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series

### Candidates with psa ≥ 40
- Scaffold `OCC(O)C(O)C(O)C(=O)N` (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

Neither scaffold is m05's scaffold (`COc1ccc(cc1)C(=O)N`), so any molecule from these with QED ≥ 0.6 works.

### Measurements taken
| mol_id | scaffold | psa | QED |
|--------|----------|-----|-----|
| m01    | OCC...   | 110.02 | 0.3187 |
| m02    | O=[N+]...| 72.24  | 0.6046 ✓ |
| m09    | O=[N+]...| 72.24  | 0.5826 |
| m14    | OCC...   | 110.02 | 0.3237 |

### Selected molecule: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

All three constraints satisfied.

## Budget used
Total measurements: 9 (5 for m05 series + 4 for Task 2 candidates)
Budget remaining: 3
