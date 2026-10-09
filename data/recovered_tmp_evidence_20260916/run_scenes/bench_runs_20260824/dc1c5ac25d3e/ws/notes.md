# Notes: QED Prediction for m05 and Lead Selection

## Task Summary
- Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - cannot measure directly
- Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in m05's series

## Step 1: Identify m05's congeneric series

Used describe_structure to identify molecules sharing the same scaffold as m05.

Target m05: scaffold = `COc1ccc(cc1)C(=O)N`, chain_len = 11

Congeners in the pool (same scaffold, different chain lengths):
- m15: chain_len = 2
- m24: chain_len = 3
- m28: chain_len = 4
- m17: chain_len = 5
- m22: chain_len = 6

## Step 2: Measure QED for congeners

Measured QED for all 5 congeners:
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

Pattern: QED peaks at chain length 3, then declines.

## Step 3: Fit trend and extrapolate to m05 (chain_len=11)

Used fit_trend with degree=2 (quadratic) as instructed not to assume linear relationship.

Quadratic fit coefficients (low to high): [0.74932, 0.017024, -0.003264]
RMSE: 0.01188

Prediction for chain_len=11: **QED = 0.5416**

Also tried linear fit (degree=1): prediction = 0.695, RMSE = 0.01308
The quadratic has lower RMSE and better captures the peak-then-decline pattern.

## Step 4: Find backup lead compound

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free check)
3. NOT in m05's series (scaffold != `COc1ccc(cc1)C(=O)N`)

Used screen_candidates() - 18 molecules pass psa ≥ 40.

None of the passing candidates have the methoxy-phenyl scaffold of m05, so all 18 satisfy constraint 3.

Measured QED for candidates:
- m02: QED = 0.6046 ≥ 0.6 ✓ (psa=72.24, scaffold=nitro-phenyl amide)
- m09: QED = 0.5826 < 0.6 (failed)

**Selected lead: m02**
- QED = 0.6046 (measured)
- psa = 72.24 ≥ 40
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold

## Summary
- Target m05 predicted QED: 0.5416 (via quadratic extrapolation from congeners)
- Lead compound: m02 with measured QED 0.6046
- Congeners in m05's series: m15, m24, m28, m17, m22
