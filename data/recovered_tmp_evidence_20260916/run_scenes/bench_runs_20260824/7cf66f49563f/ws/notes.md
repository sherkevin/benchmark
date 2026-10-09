# Notes: Predicting QED of m05

## Target
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — a 4-methoxybenzamide with an 11-carbon N-alkyl chain.
- Scaffold: `COc1ccc(cc1)C(=O)N` (4-methoxybenzamide core)
- Descriptors: MW=305.46, ALogP=4.956, PSA=38.33, ROTB=12, AROM=1

## Search Strategy
Searched the pool using the scaffold substring `COc1ccc(cc1)C(=O)N` from the target's SMILES. This found **6 total matches** (including the target m05 itself) across 2 pages.

**Budget used: 2 (search) + 5 (measurements) = 7 out of 12**

## Congeneric Series Members (same scaffold, varying N-alkyl chain length)

| mol_id | N-alkyl | n (carbons) | MW     | ALogP | PSA   | ROTB | AROM | QED (measured) |
|--------|---------|-------------|--------|-------|-------|------|------|----------------|
| m15    | -NCC    | 2           | 179.22 | 1.445 | 38.33 | 3    | 1    | 0.7621         |
| m24    | -NCCC   | 3           | 193.25 | 1.835 | 38.33 | 4    | 1    | 0.7919         |
| m28    | -NCCCC  | 4           | 207.27 | 2.225 | 38.33 | 5    | 1    | 0.7518         |
| m17    | -NCCCCC | 5           | 221.30 | 2.615 | 38.33 | 6    | 1    | 0.7498         |
| m22    | -NCCCCCC| 6           | 235.33 | 3.005 | 38.33 | 7    | 1    | 0.7377         |
| **m05**| -NCCCCCCCCCCC | **11** | **305.46** | **4.956** | **38.33** | **12** | **1** | **?** |

Each CH2 adds: ΔMW ≈ 14.03, ΔALogP ≈ 0.390, ΔROTB = 1. PSA and AROM are constant.

## Observed Trend
- QED **peaks at n=3** (0.7919), then **declines monotonically** from n=3 to n=6.
- The decline per CH2: +0.030 (2→3), -0.040 (3→4), -0.002 (4→5), -0.012 (5→6).
- The overall trend is a rise-then-fall pattern, consistent with a **concave-down (inverted-U) curve**.

## Model: Least-Squares Quadratic Fit (all 5 measured points)

Fit: **QED(n) = a*n^2 + b*n + c**

Normal equations from X^T X β = X^T y with n in {2,3,4,5,6}:
- Solved: **a = -0.003264, b = +0.01702, c = 0.74932**

**QED(n) = -0.003264*n^2 + 0.01702*n + 0.74932**

### Fit Quality (predicted vs. measured)
| n | Predicted | Measured | Residual |
|---|-----------|----------|----------|
| 2 | 0.7703    | 0.7621   | +0.0082  |
| 3 | 0.7710    | 0.7919   | -0.0209  |
| 4 | 0.7652    | 0.7518   | +0.0134  |
| 5 | 0.7528    | 0.7498   | +0.0030  |
| 6 | 0.7339    | 0.7377   | -0.0038  |

Max absolute error: 0.021 (at n=3 peak). The quadratic captures the inverted-U shape with the vertex at n ~ 2.6, consistent with the observed peak at n=3.

## Extrapolation to n=11 (m05)

**QED(11) = -0.003264 * 121 + 0.01702 * 11 + 0.74932**
**= -0.39498 + 0.18727 + 0.74932**
**= 0.5416**

### Physical Sanity Check
At n=11: ALogP=4.956, ROTB=12, MW=305.46. Both ALogP (~5) and ROTB (12) are well outside typical drug-like ranges and would incur significant QED penalties. The quadratic model's prediction of a steep drop (from 0.74 at n=6 to 0.54 at n=11) is physically reasonable -- the accelerating decline reflects the nonlinear desirability penalties for high lipophilicity and excessive flexibility.

### Why Not Linear?
A linear fit (QED = -0.00909*n + 0.79502) gives QED(11) ~ 0.695, which ignores the curvature in the data (peak at n=3) and underestimates the penalty for very high ALogP/ROTB. The task explicitly says not to assume linearity.

## Final Prediction
**QED(m05) ~ 0.5416**
