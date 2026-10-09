# Notes

## Task 1: Predict QED for m05

m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so direct measurement returns `probe_unavailable`. I predicted its QED by measuring other molecules in the same congeneric series (same scaffold `COc1ccc(cc1)C(=O)N`, differing only in N-alkyl chain length).

### Measurements in m05's scaffold series:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |
| m05    | 11        | ? (predicted) |

### Trend fitting:
- Quadratic fit (degree=2): RMSE=0.01188, prediction for chain_len=11 → **QED = 0.5416**
- Linear fit (degree=1): RMSE=0.01308, prediction for chain_len=11 → QED = 0.695

The quadratic fit has lower RMSE, so 0.5416 is the better prediction.

## Task 2: Find a lead molecule

Criteria:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in the same congeneric series as m05 (scaffold `COc1ccc(cc1)C(=O)N`)

### Molecules excluded (same series as m05 or psa < 40):
- m05 scaffold group (psa=38.33 < 40): m15, m24, m28, m17, m22, m05
- Biphenyl scaffold group (psa=29.1 < 40): m04, m08, m11, m13, m16, m20, m23, m26, m30

### Candidates with psa ≥ 40:
- Nitro scaffold (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-amide scaffold (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements for Task 2 candidates:
| mol_id | scaffold | QED | psa | meets all? |
|--------|----------|-----|-----|------------|
| m02    | nitro    | 0.6046 | 72.24 | YES ✓ |
| m09    | nitro    | 0.5826 | 72.24 | No (QED < 0.6) |
| m12    | nitro    | 0.4709 | 72.24 | No |
| m01    | sugar    | 0.3187 | 110.02 | No |
| m14    | sugar    | 0.3237 | 110.02 | No |

### Selected lead: m02
- QED = 0.6046 (≥ 0.6) ✓
- psa = 72.24 (≥ 40) ✓
- Different scaffold from m05 ✓

## Budget used: 10 measurements (5 for Task 1 series, 5 for Task 2 candidates)
