# Notes

## Task 1: Predict QED for m05

m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so I cannot measure it directly. I need to predict its QED from its congeneric series.

### Scaffold grouping
m05 belongs to scaffold "COc1ccc(cc1)C(=O)N" with chain_len=11.
Other members of this series:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Measurements for m05's series
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Polynomial fit
Used fit_trend with degree=2 to predict QED at chain_len=11:
- Prediction: **0.5416**
- RMSE: 0.01188

## Task 2: Find a lead molecule

Criteria:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same congeneric series as m05 (i.e., not scaffold COc1ccc(cc1)C(=O)N)

### Molecules with psa ≥ 40 (excluding m05's series)
- Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09, m02, m12, m32, m33, m29, m31, m06, m10 (psa=72.24)
- Scaffold 3 (OCC(O)C(O)C(O)C(=O)N): m14, m27, m01, m19, m03, m21, m18, m07, m25 (psa=110.02)
- Scaffold 4 excluded: all have psa=29.1 < 40

### Measurements for candidates
| mol_id | scaffold | psa | QED |
|--------|----------|-----|-----|
| m09 | Scaffold 2 | 72.24 | 0.5826 (fails QED ≥ 0.6) |
| m02 | Scaffold 2 | 72.24 | 0.6046 ✓ |
| m14 | Scaffold 3 | 110.02 | 0.3237 (fails) |
| m27 | Scaffold 3 | 110.02 | 0.333 (fails) |

### Selected lead molecule: m02
- QED: 0.6046 (≥ 0.6) ✓
- psa: 72.24 (≥ 40) ✓
- Scaffold 2, different from m05's Scaffold 1 ✓

## Budget used
Total measurements: 9 out of 12
- 5 for m05's series (m15, m24, m28, m17, m22)
- 4 for Task 2 candidates (m09, m02, m14, m27)
