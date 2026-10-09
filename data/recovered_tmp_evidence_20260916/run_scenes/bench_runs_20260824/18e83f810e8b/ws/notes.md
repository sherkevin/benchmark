# Notes: Predicting QED of m05

## Search Strategy
Searched for the scaffold substring `COc1ccc(cc1)C(=O)N` from the target SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. This found 6 molecules total (including m05 itself). Cost: 2 search pages.

## Congeneric Series Found
All share the scaffold `COc1ccc(cc1)C(=O)N—` and differ only in N-alkyl chain length:

| mol_id | Chain (C) | SMILES suffix | MW     | ALOGP | PSA   | ROTB | ARom |
|--------|-----------|---------------|--------|-------|-------|------|------|
| m15    | 2         | NCC           | 179.22 | 1.445 | 38.33 | 3    | 1    |
| m24    | 3         | NCCC          | 193.25 | 1.835 | 38.33 | 4    | 1    |
| m28    | 4         | NCCCC         | 207.27 | 2.225 | 38.33 | 5    | 1    |
| m17    | 5         | NCCCCC        | 221.30 | 2.615 | 38.33 | 6    | 1    |
| m22    | 6         | NCCCCCC       | 235.33 | 3.005 | 38.33 | 7    | 1    |
| **m05**| **11**    | NCCCCCCCCCCC  | 305.46 | 4.956 | 38.33 | 12   | 1    |

Descriptors increase linearly per CH₂: ΔALOGP = +0.390, ΔMW ≈ +14.03, ΔROTB = +1. PSA and ARom constant.

## QED Measurements (5 calls, budget used = 7/12 total)

| Chain (C) | mol_id | QED    |
|-----------|--------|--------|
| 2         | m15    | 0.7621 |
| 3         | m24    | 0.7919 |
| 4         | m28    | 0.7518 |
| 5         | m17    | 0.7498 |
| 6         | m22    | 0.7377 |

## Observed Trend
- QED peaks at 3C (0.7919), then declines steadily: 4C (0.7518), 5C (0.7498), 6C (0.7377).
- Step-to-step changes: +0.030 (2→3), −0.040 (3→4), −0.002 (4→5), −0.012 (5→6).
- The curve is **non-linear**: rises then falls, with an accelerating decline from 3C onward.

## Model Fitted: Quadratic (Least-Squares on All 5 Points)

I fit **QED = a·n² + b·n + c** where n = number of carbons in the N-alkyl chain.

**Normal equations solved:**
- Σn=20, Σn²=90, Σn³=440, Σn⁴=2274
- ΣQED = 3.7933, Σn·QED = 15.0823, Σn²·QED = 67.5065

**Result:**
- a = −0.003264
- b = +0.017024
- c = +0.749321

**QED(n) = −0.003264·n² + 0.017024·n + 0.749321**

**Goodness of fit (residuals):**
| n  | Predicted | Actual  | Error   |
|----|-----------|---------|---------|
| 2  | 0.7703    | 0.7621  | +0.008  |
| 3  | 0.7710    | 0.7919  | −0.021  |
| 4  | 0.7652    | 0.7518  | +0.013  |
| 5  | 0.7528    | 0.7498  | +0.003  |
| 6  | 0.7340    | 0.7377  | −0.004  |

RMS error ≈ 0.012. The vertex of the parabola is at n ≈ 2.6, consistent with the observed peak near 3C.

## Extrapolation to m05 (n = 11)

QED(11) = −0.003264 × 121 + 0.017024 × 11 + 0.749321
        = −0.39498 + 0.18726 + 0.74932
        = **0.5416**

## Sanity Check
- At 11C: ALOGP = 4.956 (near Lipinski limit), ROTB = 12 (high), MW = 305 (moderate). These are well outside typical drug-like space, so a significant QED drop from the 6C value of 0.74 is expected.
- The quadratic predicts intermediate values: 7C ≈ 0.709, 8C ≈ 0.677, 9C ≈ 0.638, 10C ≈ 0.593, 11C ≈ 0.542 — a smooth, accelerating decline consistent with nonlinear QED desirability functions.
- A log-linear model gives ≈ 0.73 (too conservative — ignores the curvature), while a linear model gives ≈ 0.70. The quadratic captures the observed non-linearity and is physically motivated (QED desirability functions are nonlinear in molecular properties, which are linear in chain length, giving a nonlinear QED-vs-chain curve).

## Predicted QED for m05: **0.54**
