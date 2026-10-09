# Notes: Predicting QED of m05

## Search Strategy
- Target molecule: m05, SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxyphenyl amide) with variable N-alkyl chain
- Searched pool using scaffold substring `COc1ccc(cc1)C(=O)N`
- Found 6 total matches across 2 pages (budget: 2 units)

## Congeneric Series Identified

| mol_id | Chain (C) | MW     | AlogP | rotb |
|--------|-----------|--------|-------|------|
| m15    | 2         | 179.22 | 1.445 | 3    |
| m24    | 3         | 193.25 | 1.835 | 4    |
| m28    | 4         | 207.27 | 2.225 | 5    |
| m17    | 5         | 221.30 | 2.615 | 6    |
| m22    | 6         | 235.33 | 3.005 | 7    |
| **m05**| **11**    | 305.46 | 4.956 | 12   |

Congeners measured: **m15, m24, m28, m17, m22**

## QED Measurements (5 budget units)

| n (chain C) | QED     |
|-------------|---------|
| 2 (m15)     | 0.7621  |
| 3 (m24)     | 0.7919  |
| 4 (m28)     | 0.7518  |
| 5 (m17)     | 0.7498  |
| 6 (m22)     | 0.7377  |

## Trend Analysis

- QED peaks at n=3 (0.7919), then decreases monotonically.
- The decrease accelerates: the underlying properties (AlogP, rotb) change linearly but QED desirability penalties are nonlinear and compound.
- AlogP increases by 0.39 per CH2 (from 1.445 at n=2 to 3.005 at n=6; target is 4.956 at n=11).
- Rotb increases by 1 per CH2 (from 3 at n=2 to 7 at n=6; target is 12 at n=11).
- At n=11, both AlogP (~5.0) and rotb (12) are well into the QED penalty zones.

## Fitted Model: Quadratic Regression (all 5 points)

Using centered variable x = n - 4, least-squares quadratic fit:

**QED(n) = -0.003264*(n-4)^2 - 0.00909*(n-4) + 0.765188**

### Fit residuals:
| n  | Predicted | Actual | Error   |
|----|-----------|--------|---------|
| 2  | 0.7703    | 0.7621 | +0.0082 |
| 3  | 0.7710    | 0.7919 | -0.0209 |
| 4  | 0.7652    | 0.7518 | +0.0134 |
| 5  | 0.7528    | 0.7498 | +0.0030 |
| 6  | 0.7340    | 0.7377 | -0.0037 |

RMSE ~ 0.015 (within training range).

### Why quadratic:
1. The negative quadratic coefficient captures accelerating QED decay as AlogP and rotb enter penalty zones.
2. Uses all 5 data points (most statistically robust).
3. Physically motivated: QED desirability functions are sigmoidal, and the product of multiple decaying desirabilities produces accelerating overall decay.
4. Linear extrapolation (0.70) and logarithmic (0.73) models incorrectly assume constant or decaying marginal penalties.

## Extrapolation to m05 (n=11)

QED(11) = -0.003264 * 49 - 0.00909 * 7 + 0.765188
        = -0.159936 - 0.063630 + 0.765188
        = **0.5416**

**Predicted QED for m05: 0.54**

## Budget Used
- search_pool: 2 pages = 2 units
- measure_qed: 5 molecules = 5 units  
- **Total: 7 of 12 units**
