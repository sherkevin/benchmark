# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - never synthesized
- Pick a backup lead compound with: QED >= 0.6, psa >= 40, NOT in m05's series
- Budget: 17 measurements total

## Strategy
1. First, list the pool to see available molecules
2. Identify m05's scaffold and congeners
3. Measure congeners to fit a trend
4. Extrapolate to predict m05's QED
5. Find a backup lead meeting all constraints

## Progress

### Step 1: List pool molecules - Done (33 molecules, 9 pages)

### Step 2: Identify m05's congeners

m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
Scaffold: COc1ccc(cc1)C(=O)N, chain_len=11

Congeners in pool (same scaffold):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Step 3: Measure QED for congeners to fit trend

Measured congeners:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

### Step 4: Fit trend and extrapolate to chain_len=11

Trend fitting results:
- Degree 1 (linear): prediction=0.695, RMSE=0.01308
- Degree 2 (quadratic): prediction=0.5416, RMSE=0.01188
- Degree 3 (cubic): prediction=2.1323 (unrealistic, >1), RMSE=0.00835

The quadratic fit has the best balance - lowest RMSE among realistic fits.
The data shows QED peaks at chain_len=3 then decreases, which a quadratic captures well.

**Predicted QED for m05 (chain_len=11): 0.5416**

### Step 5: Find backup lead compound

Constraints: QED >= 0.6, psa >= 40, NOT in m05's series

Screened 18 candidates with psa >= 40. None are in m05's series (m05 series molecules have psa=38.33 < 40).

Measured QED for candidates:
- m01: QED=0.3187 (fails QED >= 0.6)
- m02: QED=0.6046 (PASSES all constraints!)
  - QED=0.6046 >= 0.6 ✓
  - psa=72.24 >= 40 ✓
  - scaffold=O=[N+]([O-])c1ccc(cc1)C(=O)N ≠ m05's scaffold ✓
- m03: QED=0.3282 (fails QED >= 0.6)

**Selected lead: m02 with QED=0.6046**

### Summary
- Target m05 predicted QED: 0.5416
- Lead compound: m02
- Lead QED (measured): 0.6046
- Congeners in m05 series: m15, m24, m28, m17, m22
