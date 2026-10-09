# Notes

## Job 1: Predict QED of m05

### Identifying congeners
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len=11.

Used `describe_structure` to find all pool molecules with the same scaffold:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Measuring QED of congeners
Measured all 5 congeners (used 5 budget):
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Fitting trend and extrapolating
The QED peaks at chain_len=3 (0.7919) then decreases steadily. Tested different polynomial degrees:

- **Degree 1 (linear)**: prediction=0.695, RMSE=0.01308
- **Degree 2 (quadratic)**: prediction=0.5416, RMSE=0.01188
- **Degree 3 (cubic)**: prediction=2.1323 (invalid, >1), RMSE=0.00835

The cubic fit overfits and gives an impossible QED > 1. The quadratic fit has the best RMSE among valid fits and captures the curvature (peak at chain_len~3, then decline). 

**Final prediction for m05 (chain_len=11): QED = 0.5416**

The quadratic coefficients (low to high): [0.74932, 0.017024, -0.003264]
This represents: QED = 0.74932 + 0.017024*x - 0.003264*x²

## Job 2: Pick backup lead compound

### Constraints
1. measured QED >= 0.6
2. psa >= 40 (free check)
3. not in same series as m05 (scaffold != `COc1ccc(cc1)C(=O)N`)

### Screening
Used `screen_candidates()` - 18 molecules pass psa >= 40.

All m05 series molecules (m15, m17, m22, m24, m28) have psa=38.33 < 40, so none appear in the passing list. All 18 candidates automatically satisfy constraint 3.

### Measuring candidates
Measured 3 candidates to find one with QED >= 0.6:
- m01: QED=0.3187 (fails constraint 1)
- m02: QED=0.6046 ✓ (passes all constraints)
- m09: QED=0.5826 (fails constraint 1)

### Selected lead: m02
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

**Final lead: m02 with measured QED = 0.6046**

## Budget used
- 5 measurements for m05 congeners (m15, m24, m28, m17, m22)
- 3 measurements for lead candidates (m01, m02, m09)
- Total: 8/12 measurements used
