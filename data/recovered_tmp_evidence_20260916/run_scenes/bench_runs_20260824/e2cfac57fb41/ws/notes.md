# Notes: Predicting QED of m05

## Target
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 4-methoxybenzamide, N-undecyl (11-carbon chain)
- Never synthesized → must predict QED from congeneric trend

## Search Strategy
- Scaffold substring: `COc1ccc(cc1)C(=O)N` (the 4-methoxybenzamide core)
- Found 6 matches total (1 target + 5 pool congeners), retrieved in 2 pages (budget cost: 2)

## Congeneric Series (all share same scaffold, differ only in N-alkyl chain length)

| mol_id | N-alkyl | Chain length (n) | MW     | ALogP | PSA   | RotB | QED (measured) |
|--------|---------|-------------------|--------|-------|-------|------|----------------|
| m15    | ethyl   | 2                 | 179.22 | 1.445 | 38.33 | 3    | 0.7621         |
| m24    | propyl  | 3                 | 193.25 | 1.835 | 38.33 | 4    | 0.7919         |
| m28    | butyl   | 4                 | 207.27 | 2.225 | 38.33 | 5    | 0.7518         |
| m17    | pentyl  | 5                 | 221.30 | 2.615 | 38.33 | 6    | 0.7498         |
| m22    | hexyl   | 6                 | 235.33 | 3.005 | 38.33 | 7    | 0.7377         |
| **m05**| undecyl | **11**            | 305.46 | 4.956 | 38.33 | 12   | **?**          |

## Observed Trend
- QED **peaks at chain length 3** (m24, QED = 0.7919), then **declines monotonically**
- Decline accelerates as chain grows
- Physically: ALogP rises ~0.39/C and rotatable bonds increase 1/C
- At C11: ALogP = 4.956, rotb = 12 — both deep in QED penalty zone
- PSA, HBA, HBD, aromatic ring count are constant across the series

## Model: Quadratic Least-Squares Fit
Used all 5 measured points to fit QED(n) = a*n^2 + b*n + c.

Coefficients:
- a = -0.003264
- b = +0.01702
- c = +0.7493

**QED(n) = -0.003264*n^2 + 0.01702*n + 0.7493**

### Fit quality (residuals):
| n | Measured | Predicted | Residual |
|---|----------|-----------|----------|
| 2 | 0.7621   | 0.7703    | +0.008   |
| 3 | 0.7919   | 0.7710    | -0.021   |
| 4 | 0.7518   | 0.7652    | +0.013   |
| 5 | 0.7498   | 0.7528    | +0.003   |
| 6 | 0.7377   | 0.7339    | -0.004   |

RMS residual ~ 0.012

### Why quadratic?
- Task says "do not assume the relationship is a straight line"
- QED desirability functions penalize ALogP and rotb nonlinearly → decline accelerates
- Quadratic is the simplest model capturing this acceleration
- Peak at n ~ 2.6 matches the observed C3 maximum

## Extrapolation to m05 (n = 11)
QED(11) = -0.003264 * 121 + 0.01702 * 11 + 0.7493
        = -0.3949 + 0.1872 + 0.7493
        = **0.5416**

### Sanity check
- At C11: ALogP = 4.956 (very lipophilic) and rotb = 12 (very flexible) → heavily penalized
- MW = 305.46 still in acceptable range → moderate penalty
- PSA = 38.33, HBA = 2, HBD = 1, arom = 1 → all good, keeping QED well above zero
- Prediction ~0.54 is chemically reasonable for these properties

## Budget Usage
- 2 search pages + 5 QED measurements = 7 of 12 budget units used
