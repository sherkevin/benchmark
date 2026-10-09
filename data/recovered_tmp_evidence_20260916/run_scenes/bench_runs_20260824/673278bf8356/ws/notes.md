# Notes: QED Prediction for m05 and Lead Selection

## Task Overview
- Predict QED for m05 (never synthesized, chain_len=11) using congeneric series extrapolation
- Select a backup lead compound with: QED >= 0.6, psa >= 40, NOT in m05's series

## Step 1: Identify m05's Congeneric Series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len=11.

By examining SMILES patterns and verifying with `describe_structure`, I identified all pool molecules sharing the same scaffold:

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |

These 5 molecules are the congeners of m05 in the pool.

## Step 2: Measure Congeners and Fit Trend

Measured QED for all congeners:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

The data shows a peak at chain_len=3, then a gradual decline. This is clearly non-linear.

Fitted a quadratic polynomial (degree=2) through the 5 points:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188

Extrapolating to chain_len=11:
- **Predicted QED for m05 = 0.5416**

(For comparison: linear fit gave 0.695 with higher RMSE=0.01308; cubic fit gave absurd 2.1323 due to overfitting with only 5 points.)

## Step 3: Select Backup Lead Compound

Constraints:
1. measured QED >= 0.6
2. psa >= 40 (free check via screen_candidates)
3. NOT in m05's series (scaffold != `COc1ccc(cc1)C(=O)N`)

From `screen_candidates()`, 18 molecules pass psa >= 40. None of these are in m05's series (the m05 congeners all have psa=38.33, below the threshold).

With limited measurement budget (17 total, 5 used for congeners = 12 remaining), I measured:
- m01: QED = 0.3187 (fails QED >= 0.6)
- m02: QED = 0.6046 (PASSES all constraints!)
- m03: QED = 0.3282 (fails QED >= 0.6)

**Selected lead: m02**
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

## Budget Used
- 5 measurements for congeners (m15, m24, m28, m17, m22)
- 3 measurements for lead candidates (m01, m02, m03)
- Total: 8/17 measurements used

## Final Answers
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]
