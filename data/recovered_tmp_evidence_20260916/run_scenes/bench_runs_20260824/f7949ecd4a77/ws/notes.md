# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - has 11 carbons in alkyl chain
- Pick a backup lead compound with: measured QED ≥ 0.6, psa ≥ 40, NOT in same series as m05
- Budget: 17 measurements total

## Step 1: Identified congeners in m05's series
The scaffold for m05 is `COc1ccc(cc1)C(=O)N`. Congeners in the pool:
- m15: chain_len = 2, QED = 0.7621
- m24: chain_len = 3, QED = 0.7919
- m28: chain_len = 4, QED = 0.7518
- m17: chain_len = 5, QED = 0.7498
- m22: chain_len = 6, QED = 0.7377

m05 has chain_len = 11.

## Step 2: Trend fitting for m05 prediction
Quadratic fit (degree=2, RMSE=0.01188):
- coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
- Prediction at x=11: QED = 0.5416

Linear fit (degree=1, RMSE=0.01308):
- Prediction at x=11: QED = 0.695

The quadratic fit is better (lower RMSE) and captures the peak at chain_len=3 followed by decline.
Using quadratic prediction: **m05 QED ≈ 0.5416**

## Step 3: Finding backup lead compound
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free check)
3. NOT in same series as m05 (scaffold ≠ "COc1ccc(cc1)C(=O)N")

Screen candidates with psa ≥ 40: 18 molecules passed.
None of these are in m05's series (m05 series has psa=38.33, below threshold).

Measurements for lead selection:
- m02: QED = 0.6046 ✓ (passes all constraints)
  - psa = 72.24 ≥ 40 ✓
  - scaffold = "O=[N+]([O-])c1ccc(cc1)C(=O)N" ≠ m05's scaffold ✓
- m12: QED = 0.4709 (fails QED constraint)
- m01: QED = 0.3187 (fails QED constraint)

**Selected lead: m02** with measured QED = 0.6046

## Summary
- target_qed: 0.5416 (predicted via quadratic extrapolation)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]
