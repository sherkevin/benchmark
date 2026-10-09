# Notes: QED Prediction for m05 and Lead Compound Selection

## Job 1: Predict QED for m05

### Identifying the Congeneric Series
Target molecule m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len=11.

I identified all pool molecules sharing the same scaffold (same congeneric series):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### QED Measurements
Measured QED for all 5 congeners:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend Fitting and Extrapolation
Fitted a quadratic polynomial (degree=2) through the 5 data points:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188

Extrapolated to chain_len=11:
- **Predicted QED for m05: 0.5416**

Also tried linear fit (degree=1) for comparison:
- Prediction: 0.695, RMSE: 0.01308

The quadratic fit has lower RMSE and the task explicitly says not to assume a straight line, so I use the quadratic prediction of 0.5416.

## Job 2: Select Backup Lead Compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (free check)
3. not in the same series as m05 (free check via SMILES)

### Screening
Used `screen_candidates()` to find molecules with psa ≥ 40. Found 18 candidates.

None of the m05-series molecules (m15, m17, m22, m24, m28) pass the psa screen since they all have psa=38.33 < 40. So all 18 candidates automatically satisfy constraint 3.

### QED Measurements for Lead Selection
Measured QED for candidates to find one with QED ≥ 0.6:
- m02: QED = 0.6046 ✓ (passes all constraints)
- m09: QED = 0.5826 ✗ (below 0.6)
- m12: QED = 0.4709 ✗ (below 0.6)

### Selected Lead
**m02** is the chosen lead compound:
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-phenyl-amide, different from m05)
- chain_len: 3
- psa: 72.24 (≥ 40 ✓)
- measured QED: 0.6046 (≥ 0.6 ✓)

## Budget Used
- 5 measurements for congeneric series (m15, m24, m28, m17, m22)
- 3 measurements for lead screening (m02, m09, m12)
- Total: 8 out of 17 budget
