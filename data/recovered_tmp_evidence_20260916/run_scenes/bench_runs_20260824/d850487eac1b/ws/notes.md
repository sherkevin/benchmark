# Notes

## Task 1: Predict QED for m05

m05 is in scaffold group 1 (COc1ccc(cc1)C(=O)N) with chain_len=11.

Measured QED for other molecules in the same series:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

Used fit_trend with degree=2 (quadratic) to predict QED at chain_len=11:
- Prediction: 0.5416
- RMSE: 0.01188

Linear fit (degree=1) gave 0.695 with RMSE 0.01308.
Cubic fit (degree=3) gave unrealistic 2.1323 (overfitting).

**Selected prediction for m05: 0.5416** (quadratic fit, best RMSE among reasonable models)

## Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series

Scaffold groups:
- Scaffold 1 (m05's series): psa=38.33 - EXCLUDED (also psa < 40)
- Scaffold 2: psa=72.24 ≥ 40 - candidates: m09, m02, m12, m32, m33, m29, m31, m06, m10
- Scaffold 3: psa=110.02 ≥ 40 - candidates: m14, m27, m01, m19, m03, m21, m18, m07, m25
- Scaffold 4: psa=29.1 < 40 - EXCLUDED

Measured QED for candidates:
- m01 (scaffold 3): QED=0.3187 (< 0.6, fails)
- m02 (scaffold 2): QED=0.6046 (≥ 0.6, PASSES!)
- m14 (scaffold 3): QED=0.3237 (< 0.6, fails)

**Selected: m02**
- QED: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold 2, not m05's scaffold 1 (✓)

## Budget usage
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements (m15, m24, m28, m17, m22, m01, m02, m14) = 8 units
- Total: 17/17 budget used
