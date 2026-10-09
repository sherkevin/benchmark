# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - cannot measure directly
- Pick a backup lead compound with: QED >= 0.6, psa >= 40, NOT in m05's series

## Budget
- 17 total measurements for measure_qed
- Free tools: fit_trend, screen_candidates, describe_structure, list_pool

## Step 1: Identified congeners of m05

m05 has scaffold `COc1ccc(cc1)C(=O)N` (methoxy-benzamide) with chain_len=11.

Found congeners in the pool (same scaffold):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

## Step 2: Measured QED of congeners

Measurements used: 5 (budget: 10-14)
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

## Step 3: Fit trend and extrapolate to chain_len=11

Tried different polynomial degrees:
- Degree 1 (linear): prediction = 0.695, RMSE = 0.01308
- Degree 2 (quadratic): prediction = 0.5416, RMSE = 0.01188
- Degree 3 (cubic): prediction = 2.1323 (invalid, >1), RMSE = 0.00835

The quadratic fit is most appropriate:
- Task says "do not assume the relationship is a straight line"
- Cubic overfits and gives impossible extrapolation (>1)
- Quadratic has lowest RMSE among valid fits

**Final prediction for m05 QED: 0.5416**

Quadratic coefficients (low to high): [0.74932, 0.017024, -0.003264]
This represents: QED = 0.74932 + 0.017024*x - 0.003264*x^2

## Step 4: Find backup lead compound

Constraints:
1. measured QED >= 0.6
2. psa >= 40 (free check)
3. NOT in m05 series

Used screen_candidates() - 18 molecules pass psa >= 40.

None of the m05 series congeners pass the psa screen (they all have psa=38.33 < 40), so all 18 candidates automatically satisfy constraint 3.

Measured QED for candidates (used 3 more measurements, total 17):
- m02: QED = 0.6046 ✓ (passes all constraints)
- m09: QED = 0.5826 (fails QED >= 0.6)
- m12: QED = 0.4709 (fails QED >= 0.6)

**Selected lead: m02**
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide) - different from m05's methoxy-benzamide ✓

## Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]
