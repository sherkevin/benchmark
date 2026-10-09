# Notes

## Task Summary
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - chain_len=11
- Pick a backup lead compound with: QED >= 0.6, psa >= 40, NOT in same series as m05
- Budget: 17 measurements total

## Step 1: Identify congeners of m05

m05 has scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11.

Congeners in the pool (same scaffold):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

## Step 2: Measure QED for all congeners to establish trend

Measured QED values:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

## Step 3: Fit trend and extrapolate to chain_len=11

Used fit_trend with degree=2 (quadratic):
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query = 11

Result: prediction = 0.5416
- RMSE = 0.01188
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]

The quadratic fit captures the peak at chain_len=3 and subsequent decline. 
Degree=3 gave nonsensical result (QED > 1), indicating overfitting.
Degree=1 (linear) gave 0.695, but task says not to assume straight line.

**Predicted QED for m05: 0.5416**

## Step 4: Find backup lead compound

Constraints:
1. measured QED >= 0.6
2. psa >= 40 (free check via screen_candidates)
3. NOT in m05 series

screen_candidates returned 18 molecules with psa >= 40. None are in m05 series 
(m05 congeners have psa=38.33 < 40, so they don't appear in the list).

Measured QED for candidates:
- m01: QED=0.3187 (fails constraint 1)
- m02: QED=0.6046 (PASSES all constraints!)
- m09: QED=0.5826 (fails constraint 1)

**Selected lead: m02**
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (nitrobenzamide) ≠ m05 scaffold ✓

## Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
