# Notes for QED Prediction Task

## Task Summary
- Predict QED for molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) which has never been synthesized
- Pick a backup lead compound with: measured QED ≥ 0.6, psa ≥ 40, NOT in same series as m05
- Budget: 17 measurements total

## Analysis of Compound Pool

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl amide (COc1ccc(cc1)C(=O)N-)
- Chain: 11 carbons (CCCCCCCCCCC)

### Identifying the Congeneric Series (same scaffold, different chain lengths)

Looking through all 33 molecules, I found these congeners with the same `COc1ccc(cc1)C(=O)N-` scaffold:

| mol_id | Chain Length | QED (measured) |
|--------|--------------|----------------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |
| m05 | 11 | ??? (target) |

Congeners in m05 series: m15, m24, m28, m17, m22 (5 molecules)

## Trend Fitting for m05 QED Prediction

Data points: chain lengths [2, 3, 4, 5, 6] → QED [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

The trend shows:
- QED peaks at chain length 3 (0.7919)
- Then decreases monotonically: 0.7518 → 0.7498 → 0.7377

Fitted quadratic polynomial (degree=2):
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188
- Prediction for x=11: **0.5416**

Linear fit (degree=1) gave 0.695 with higher RMSE (0.01308).
Cubic fit (degree=3) gave 2.13 which is outside valid QED range (overfitting).

**Final prediction for m05 QED: 0.5416** (using quadratic fit, which best captures the parabolic trend)

## Backup Lead Selection

Screened candidates with psa ≥ 40: 18 molecules passed.

None of the m05 series congeners pass the psa screen (they all have psa=38.33 < 40), so all 18 are automatically not in the m05 series.

Measured QED for candidates:
- m01: QED = 0.3187 (fails, < 0.6)
- m02: QED = 0.6046 ✓ (passes all constraints!)
- m03: QED = 0.3282 (fails, < 0.6)

**Selected lead: m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- psa: 72.24 (≥ 40) ✓
- Not in m05 series (nitro-phenyl scaffold, not methoxy-phenyl) ✓
- Measured QED: 0.6046 (≥ 0.6) ✓

## Budget Usage
- list_pool: 9 pages = 9 measurements
- measure_qed: 5 congeners + 3 candidates = 8 measurements
- Total: 17/17 (full budget used)
