# Notes: QED Prediction for m05

## Target
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 4-methoxybenzamide with an 11-carbon N-alkyl chain
- MW=305.46, ALOGP=4.956, PSA=38.33, ROTB=12, AROM=1
- Cannot be measured (never synthesized); must predict QED from congeneric series.

## Search Strategy
Searched the pool using the scaffold substring `COc1ccc(cc1)C(=O)N` from the target SMILES. This found 6 total matches across 2 pages (cost: 2 budget units).

## Congeneric Series Found
All share the scaffold `COc1ccc(cc1)C(=O)N-` and differ only in N-alkyl chain length:

| mol_id | Chain carbons | MW | ALOGP | ROTB | QED (measured) |
|--------|--------------|-------|-------|------|---------------|
| m15 | 2 | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | 3 | 193.25 | 1.835 | 4 | 0.7919 |
| m28 | 4 | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | 5 | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | 6 | 235.33 | 3.005 | 7 | 0.7377 |
| **m05** | **11** | 305.46 | 4.956 | 12 | **?** |

5 measurements used 5 budget units. Total budget used: 7 of 12.

## Observed Trend
QED peaks at chain length C3 (0.7919), then declines monotonically:
C2=0.7621, C3=0.7919, C4=0.7518, C5=0.7498, C6=0.7377.

The decline is driven by increasing MW, ALOGP, and ROTB pushing past optimal drug-like ranges.

## Model: Quadratic fit on all 5 data points
Fitting QED = a*n^2 + b*n + c by least squares:
- a = -0.003264
- b = +0.017024  
- c = +0.749321

Equation: QED(n) = -0.003264*n^2 + 0.017024*n + 0.749321

Fit quality (predicted vs actual):
- C2: 0.770 vs 0.762
- C3: 0.771 vs 0.792
- C4: 0.765 vs 0.752
- C5: 0.753 vs 0.750
- C6: 0.734 vs 0.738

## Cross-checks with other models:
- Quadratic on C4-C6 only: QED(11) = 0.526
- Log-space quadratic (all 5): QED(11) = 0.570
- Linear all 5: QED(11) = 0.695 (rejected per task instructions)

## Extrapolation
QED(11) = -0.003264(121) + 0.017024(11) + 0.749321 = 0.542

The three non-linear models give: 0.526, 0.542, 0.570 — consistent range.

## Physical Reasonableness
At C11: MW=305, ALOGP=4.96, ROTB=12 — all sub-optimal for drug-likeness.
ALOGP near 5 and ROTB=12 justify significant QED drop from 0.74 (C6) to ~0.54.

## Final Prediction: QED of m05 = 0.54
Congener IDs: m15, m24, m28, m17, m22
