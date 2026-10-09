# Notes: Predicting QED of m05

## Step 1: Identify the congeneric series for m05

Target m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
Scaffold: 4-methoxybenzamide (COc1ccc(cc1)C(=O)N-), variable: N-alkyl chain length.

Congeners in pool (same scaffold, differing chain length):
- m15: chain=2 (NCC)
- m24: chain=3 (NCCC)
- m28: chain=4 (NCCCC)
- m17: chain=5 (NCCCCC)
- m22: chain=6 (NCCCCCC)
- m05: chain=11 (NCCCCCCCCCCC) -- TARGET

All share PSA=38.33, AROM=1, identical HBA/HBD/alerts. Only MW (+14/C), ALOGP (+0.39/C), ROTB (+1/C) change.

## Step 2: Measurements (8 total budget)

### Target series (5 measurements):
| ID  | Chain C | QED    |
|-----|---------|--------|
| m15 | 2       | 0.7621 |
| m24 | 3       | 0.7919 |
| m28 | 4       | 0.7518 |
| m17 | 5       | 0.7498 |
| m22 | 6       | 0.7377 |

### Cross-validation from other series (3 measurements):
| ID  | Series   | Chain C | QED    |
|-----|----------|---------|--------|
| m20 | biphenyl | 7       | 0.678  |
| m08 | biphenyl | 9       | 0.5472 |
| m10 | nitro    | 10      | 0.3724 |

## Step 3: Trend analysis

QED peaks at C3 (0.792), then declines:
- C2->C3: +0.030 (entering optimal ALOGP range)
- C3->C4: -0.040 (ALOGP/ROTB begin penalizing)
- C4->C5: -0.002 (shallow region)
- C5->C6: -0.012 (decline re-accelerates)

Non-monotonic derivative and accelerating decline after C4 rules out linear model.

## Step 4: Quadratic fit

Using centered variable t = chain_length - 4, least-squares on all 5 target series points:

Normal equations:
- n=5, sum_t=0, sum_t2=10, sum_t4=34
- sum_y=3.7933, sum_ty=-0.0909, sum_t2y=7.5409

Solution:
- A = 0.7652 (constant term at t=0, i.e., chain C4)
- B = -0.00909 (linear coefficient)
- C = -0.003264 (quadratic coefficient, negative = concave down)

**QED(n) = 0.7652 - 0.00909*(n-4) - 0.003264*(n-4)^2**

Residuals: C2: +0.008, C3: -0.021, C4: +0.013, C5: +0.003, C6: -0.004. RMS ~0.012.

## Step 5: Extrapolation to m05 (n=11, t=7)

QED = 0.7652 - 0.00909*7 - 0.003264*49
QED = 0.7652 - 0.0636 - 0.1599
**QED = 0.542**

## Sanity checks:
- Biphenyl C7 (ALOGP=5.05, ROTB=8): QED=0.678. Our m05 has similar ALOGP (4.96) but more ROTB (12) and fewer aromatics (1 vs 2), so lower QED is expected.
- Biphenyl C9 (ALOGP=5.83, ROTB=10): QED=0.547. Similar QED level despite higher ALOGP because fewer ROTB and more aromatics compensate.
- The predicted per-carbon drop at C10->C11 is ~0.05, consistent with the accelerating quadratic trend.

## Conclusion
Predicted QED of m05: **0.542**
