# Notes: QED Prediction for m05 and Lead Compound Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain length 11.

I identified all pool molecules sharing the same scaffold by calling `describe_structure`:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

These 5 molecules form the congeneric series with m05.

### Measuring QED Values
I measured QED for all 5 congeners:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Fitting the Trend
The data shows a peak at chain_len=3, then a gradual decline. This is not linear.

**Quadratic fit (degree=2):**
- Coefficients (low to high): 0.74932, 0.017024, -0.003264
- RMSE: 0.01188
- Prediction at x=11: **0.5416**

**Linear fit (degree=1) for comparison:**
- Prediction at x=11: 0.695
- RMSE: 0.01308

The quadratic fit has lower RMSE and better captures the curvature (peak at chain_len=3, then decline). The task explicitly says "do not assume the relationship is a straight line", so I use the quadratic prediction.

**Final predicted QED for m05: 0.5416**

## Job 2: Selecting a Backup Lead Compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (free check)
3. not in the same series as m05

### Screening by PSA
18 molecules passed the psa ≥ 40 filter: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33.

None of the m05 series congeners (m15, m17, m22, m24, m28) appear in this list because they have psa=38.33 < 40.

### Measuring QED for Candidates
I measured QED for several candidates:
- m01: QED=0.3187 (fails, < 0.6)
- m02: QED=0.6046 ✓ (passes!)
- m03: QED=0.3282 (fails)
- m06: QED=0.4011 (fails)
- m07: QED=0.3233 (fails)

### Selected Lead: m02
- **measured QED**: 0.6046 (≥ 0.6) ✓
- **psa**: 72.24 (≥ 40) ✓
- **scaffold**: `O=[N+]([O-])c1ccc(cc1)C(=O)N` — different from m05's scaffold ✓

m02 satisfies all three constraints and is selected as the backup lead compound.

## Budget Usage
- 5 measurements for congeneric series (m15, m24, m28, m17, m22)
- 5 measurements for lead candidates (m01, m02, m03, m06, m07)
- Total: 10 out of 12 budget used
