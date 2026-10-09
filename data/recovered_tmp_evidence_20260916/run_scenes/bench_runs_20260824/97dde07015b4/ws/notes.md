# Notes on QED Prediction and Lead Selection

## Task Overview
1. Predict QED for m05 (unmeasurable, never synthesized)
2. Pick a backup lead compound with: measured QED >= 0.6, psa >= 40, not in m05's series

## Identifying m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxy-phenyl-amide)
- Chain: 13 carbons (CCCCCCCCCCC)

Molecules in the same series (same scaffold, different chain lengths):
- m15: `COc1ccc(cc1)C(=O)NCC` - 2 carbons
- m24: `COc1ccc(cc1)C(=O)NCCC` - 3 carbons  
- m28: `COc1ccc(cc1)C(=O)NCCCC` - 4 carbons
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5 carbons
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6 carbons
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - 13 carbons (TARGET)

Chain lengths: 2, 3, 4, 5, 6, 13

## Plan
1. Measure QED for all available congeners (m15, m24, m28, m17, m22)
2. Fit a polynomial trend to QED vs chain length
3. Extrapolate to chain length 13 to predict m05's QED
4. For lead selection: screen by psa >= 40, exclude m05 series, measure QED for candidates

## Measurements Taken for m05 Series

| mol_id | chain_length | QED |
|--------|--------------|-----|
| m15    | 2            | 0.7621 |
| m24    | 3            | 0.7919 |
| m28    | 4            | 0.7518 |
| m17    | 5            | 0.7498 |
| m22    | 6            | 0.7377 |

## Trend Fitting

Fit quadratic polynomial through points (chain_length, QED):
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Result from fit_trend (degree=2, x_query=13):
- prediction = 0.419
- coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE = 0.01188

Equation: QED(x) = 0.74932 + 0.017024*x - 0.003264*x²

For x=13 (m05's chain length): QED = 0.419

## Lead Selection

Candidates passing psa >= 40: 18 molecules (m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33)

None of these are in m05's series (m15, m17, m22, m24, m28), so all pass constraint 3.

Measured QED for candidates:
- m01: 0.3187 (fail)
- m02: 0.6046 ✓ PASS (QED >= 0.6, psa = 72.24 >= 40, not in m05 series)
- m03: 0.3282 (fail)
- m09: 0.5826 (fail)
- m14: 0.3237 (fail)
- m12: 0.4709 (fail)
- m27: 0.333 (fail)

Selected lead: m02 with measured QED = 0.6046

## Summary

- Predicted QED for m05: 0.419
- Congeners in m05 series: m15, m24, m28, m17, m22
- Lead compound: m02 (measured QED = 0.6046)
