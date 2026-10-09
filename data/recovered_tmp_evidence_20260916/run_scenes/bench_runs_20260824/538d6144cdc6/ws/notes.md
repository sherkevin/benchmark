# Notes

## Strategy
1. Used `group_by_scaffold()` (free) to identify 4 congeneric series
2. m05 is in series 1 (scaffold `COc1ccc(cc1)C(=O)N`) with chain_len=11
3. Measured QED for all 5 other members of m05's series (chain_len 2-6)
4. Fitted polynomial to predict m05's QED at chain_len=11
5. Searched pool for Task 2 candidates from other series with PSA ≥ 40

## Task 1: Predicting m05's QED

### Congeneric series data (scaffold: COc1ccc(cc1)C(=O)N):
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Fits:
- **Degree 1 (linear)**: QED(11) = 0.695, RMSE = 0.01308
- **Degree 2 (quadratic)**: QED(11) = 0.5416, RMSE = 0.01188
  - Coefficients: [0.74932, 0.017024, -0.003264]
  - Negative quadratic term captures expected decrease in QED with longer chains

Used degree-2 prediction: **0.5416** (better RMSE, physically more reasonable for extrapolation)

## Task 2: Finding a lead molecule

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series (group 1)

### Scaffold groups:
- Group 1 (m05's): COc1ccc(cc1)C(=O)N — excluded
- Group 2: O=[N+]([O-])c1ccc(cc1)C(=O)N — PSA ≈ 72.24 ✓
- Group 3: OCC(O)C(O)C(O)C(=O)N — PSA ≈ 110.02 ✓
- Group 4: c1ccc(cc1)-c1ccc(cc1)C(=O)N — PSA ≈ 29.1 ✗ (fails PSA)

### Measurements for Task 2 candidates:
| mol_id | group | chain_len | QED    | PSA    | Pass? |
|--------|-------|-----------|--------|--------|-------|
| m09    | 2     | 2         | 0.5826 | 72.24  | QED < 0.6 ✗ |
| m14    | 3     | 2         | 0.3237 | 110.02 | QED < 0.6 ✗ |
| m02    | 2     | 3         | 0.6046 | 72.24  | ALL ✓ |
| m12    | 2     | 4         | 0.4709 | 72.24  | QED < 0.6 ✗ |

### Selected: **m02**
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Group 2 (not m05's series) ✓
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC

## Budget usage: 12/12
- 5 measurements for m05's series (m15, m24, m28, m17, m22)
- 3 pages listed (pages 1-3)
- 4 measurements for Task 2 candidates (m09, m14, m02, m12)
- Total: 5 + 3 + 4 = 12
