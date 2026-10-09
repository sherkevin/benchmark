# Notes on QED Prediction and Lead Selection

## Task Overview
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - cannot be measured directly
- Pick a backup lead: QED ≥ 0.6, psa ≥ 40, NOT in m05's series

## Step 1: Identify congeners of m05 (same scaffold)

Target m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N- (methoxy-phenyl amide)
- N-alkyl chain: 11 carbons

Verified with describe_structure:
- m05: scaffold="COc1ccc(cc1)C(=O)N", chain_len=11
- m15: scaffold="COc1ccc(cc1)C(=O)N", chain_len=2
- m24: scaffold="COc1ccc(cc1)C(=O)N", chain_len=3
- m28: scaffold="COc1ccc(cc1)C(=O)N", chain_len=4
- m17: scaffold="COc1ccc(cc1)C(=O)N", chain_len=5
- m22: scaffold="COc1ccc(cc1)C(=O)N", chain_len=6

**Congeners of m05 in the pool: m15, m24, m28, m17, m22**

## Step 2: Measure QED for m05 series congeners

Measured QED values:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

## Step 3: Fit trend and extrapolate to chain_len=11

Quadratic fit (degree=2):
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query = 11
- Result: prediction = 0.5416, RMSE = 0.01188
- Coefficients: [0.74932, 0.017024, -0.003264] (low to high)

Linear fit (degree=1) for comparison:
- Result: prediction = 0.695, RMSE = 0.01308

The quadratic fit has lower RMSE and better captures the curvature (peak at chain_len=3, then decline). Using quadratic prediction: **QED(m05) = 0.5416**

## Step 4: Find backup lead compound

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05 series (scaffold ≠ "COc1ccc(cc1)C(=O)N")

Screened candidates with psa ≥ 40: 18 molecules pass.
None of the m05 series members (m15, m17, m22, m24, m28) have psa ≥ 40, so all 18 are valid candidates for constraint 3.

Measured QED for candidates:
- m01: QED=0.3187 (fails, < 0.6)
- m02: QED=0.6046 ✓ (passes all constraints!)
- m09: QED=0.5826 (fails, < 0.6)
- m14: QED=0.3237 (fails, < 0.6)

**Selected lead: m02**
- psa=72.24 ≥ 40 ✓
- QED=0.6046 ≥ 0.6 ✓
- scaffold="O=[N+]([O-])c1ccc(cc1)C(=O)N" ≠ m05's scaffold ✓

## Summary
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: [m15, m24, m28, m17, m22]
