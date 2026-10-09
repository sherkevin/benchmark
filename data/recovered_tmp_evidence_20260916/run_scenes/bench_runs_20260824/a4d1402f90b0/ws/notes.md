# Notes

## Task 1: Predict QED for m05

m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so direct measurement returns `probe_unavailable`. I predicted its QED by building a trend from its congeneric series.

### Congeneric series for m05
Scaffold: `COc1ccc(cc1)C(=O)N`
Members with chain lengths:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11 (target)

### Measurements taken for the series
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Polynomial fit
Used degree=2 polynomial fit with chain lengths as x and QED as y.
- x_query = 11 (m05's chain length)
- Predicted QED = **0.5416**
- RMSE on given points: 0.01188

## Task 2: Find a lead molecule

Constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in the same congeneric series as m05

### Molecules with psa ≥ 40 (not in m05's series)
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Scaffold `OCC(O)C(O)C(O)C(=O)N` (psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

### Measurements for Task 2 candidates
| mol_id | scaffold | psa | QED |
|--------|----------|-----|-----|
| m09 | O=[N+]([O-])c1ccc(cc1)C(=O)N | 72.24 | 0.5826 (< 0.6, fails) |
| m02 | O=[N+]([O-])c1ccc(cc1)C(=O)N | 72.24 | 0.6046 (≥ 0.6, passes) |
| m14 | OCC(O)C(O)C(O)C(=O)N | 110.02 | 0.3237 (< 0.6, fails) |
| m27 | OCC(O)C(O)C(O)C(=O)N | 110.02 | 0.3330 (< 0.6, fails) |

### Selected lead molecule: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

## Budget used
Total measurements: 9 out of 12
- 5 for m05's congeneric series (m15, m24, m28, m17, m22)
- 4 for Task 2 candidates (m09, m02, m14, m27)
