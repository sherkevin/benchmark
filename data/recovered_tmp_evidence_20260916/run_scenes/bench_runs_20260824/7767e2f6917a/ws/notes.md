# Notes on QED Prediction Task

## Target molecule m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons
- m05 has never been synthesized, so measure_qed returns probe_unavailable

## Task breakdown
1. Find all molecules in the same series as m05 (same scaffold)
2. Measure QED for those molecules to understand the trend
3. Fit a trend and extrapolate to predict QED for m05 (chain_len=11)
4. Find a backup lead compound with: measured QED >= 0.6, psa >= 40, NOT in m05's series

## Identifying congeners of m05

Molecules with scaffold `COc1ccc(cc1)C(=O)N`:
- m15: chain_len=2, QED=0.7621 (measured)
- m24: chain_len=3, QED=0.7919 (measured)
- m28: chain_len=4, QED=0.7518 (measured)
- m17: chain_len=5, QED=0.7498 (measured)
- m22: chain_len=6, QED=0.7377 (measured)

Congener IDs: [m15, m17, m22, m24, m28]

## Trend fitting for m05 prediction

Data points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)

Quadratic fit (degree=2):
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
- RMSE: 0.01188
- Prediction for x=11: **0.5416**

Linear fit (degree=1):
- Coefficients: 0.79502 - 0.00909*x
- RMSE: 0.01308
- Prediction for x=11: 0.695

The quadratic fit has lower RMSE and better captures the peak at chain=3 followed by decline. Using quadratic prediction: **target_qed = 0.5416**

## Finding backup lead compound

Constraints:
1. measured QED >= 0.6
2. psa >= 40 (free check)
3. NOT in m05's series (scaffold != `COc1ccc(cc1)C(=O)N`)

18 molecules pass psa >= 40 screen. None of m05's series members pass (they have psa=38.33).

Measurements used (budget=12):
- m01: QED=0.3187 (fails, < 0.6)
- m02: QED=0.6046 (PASSES all constraints!)
- m09: QED=0.5826 (fails, < 0.6)
- m14: QED=0.3237 (fails, < 0.6)
- m32: QED=0.4683 (fails, < 0.6)

**Selected lead: m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05's scaffold)
- psa: 72.24 >= 40 ✓
- measured QED: 0.6046 >= 0.6 ✓
- Not in m05's series ✓

## Summary for answer.json
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
