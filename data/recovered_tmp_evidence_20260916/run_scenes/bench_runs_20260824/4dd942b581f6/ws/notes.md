# Notes on QED Prediction and Lead Selection

## Task Overview
- Predict QED for m05 (never synthesized, so measure_qed returns probe_unavailable)
- Pick a backup lead compound from the pool with: measured QED ≥ 0.6, psa ≥ 40, not in same series as m05
- Budget: 12 measurements total

## Step 1: Identify m05's series (congeners)

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxy-phenyl-amide)
- N-alkyl chain length: 11 carbons

Congeners in the pool (same scaffold):
- m15: chain_len = 2
- m24: chain_len = 3
- m28: chain_len = 4
- m17: chain_len = 5
- m22: chain_len = 6

m05 has chain_len = 11, so I need to extrapolate from shorter chains (2-6) to predict QED at chain_len = 11.

## Step 2: Measure QED for all congeners

Measured QED values:
- m15: chain_len=2, QED=0.7621
- m24: chain_len=3, QED=0.7919
- m28: chain_len=4, QED=0.7518
- m17: chain_len=5, QED=0.7498
- m22: chain_len=6, QED=0.7377

The trend shows QED peaks around chain_len=3 and then decreases. Need to fit a polynomial and extrapolate to chain_len=11.

## Step 3: Fit trend and extrapolate to chain_len=11

Using fit_trend with different degrees:
- degree=1 (linear): prediction = 0.695, RMSE = 0.01308
- degree=2 (quadratic): prediction = 0.5416, RMSE = 0.01188
- degree=3 (cubic): prediction = 2.1323 (unrealistic, >1), RMSE = 0.00835

The quadratic fit is most appropriate:
- Lower RMSE than linear
- Captures the peak-and-decline pattern in the data
- Gives a realistic prediction within valid QED range [0,1]

**Predicted QED for m05: 0.5416**

## Step 4: Find backup lead compound

Criteria:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free check)
3. NOT in the same series as m05 (free check)

Screen candidates with psa ≥ 40: 18 molecules pass.

Checking scaffolds - none of the 18 candidates have scaffold `COc1ccc(cc1)C(=O)N` (m05's series).
- Series 1: `OCC(O)C(O)C(O)C(=O)N` (polyol-amide): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Series 2: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-phenyl-amide): m02, m06, m09, m10, m12, m29, m31, m32, m33

All 18 pass constraint 3 (not in m05's series). Now need to find one with QED ≥ 0.6.

Budget remaining: 12 - 5 = 7 measurements.

Measured candidates:
- m01: QED = 0.3187 (fails, < 0.6)
- m02: QED = 0.6046 (PASSES all criteria!)
- m14: QED = 0.3237 (fails, < 0.6)
- m09: QED = 0.5826 (fails, < 0.6)

**Selected lead: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

## Summary

**Target m05 QED prediction:**
- Congeners: m15, m24, m28, m17, m22 (same scaffold `COc1ccc(cc1)C(=O)N`)
- Quadratic fit on (chain_len, QED) points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
- Predicted QED at chain_len=11: 0.5416

**Lead compound: m02**
- Measured QED: 0.6046
- psa: 72.24
- Not in m05's series
