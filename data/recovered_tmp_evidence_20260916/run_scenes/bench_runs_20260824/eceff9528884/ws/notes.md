# Notes: Predicting QED of m05

## Target molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide with N-undecyl chain (11 carbons)
- MW=305.46, ALogP=4.956, PSA=38.33, RotB=12, Arom=1
- Never synthesized; QED must be predicted by extrapolation.

## Search strategy
Searched `search_pool` with the scaffold substring `COc1ccc(cc1)C(=O)N` (the constant part of the target's SMILES). Found **6 total matches** (including m05 itself) across 2 pages. Budget used: 2 units for search + 5 units for measurements = 7 of 12.

## Congeneric series (same scaffold, varying N-alkyl chain length)

| mol_id | N-alkyl chain (n carbons) | SMILES suffix | MW     | ALogP | RotB | QED (measured) |
|--------|---------------------------|---------------|--------|-------|------|-----------------|
| m15    | 2 (ethyl)                 | NCC           | 179.22 | 1.445 | 3    | 0.7621          |
| m24    | 3 (propyl)                | NCCC          | 193.25 | 1.835 | 4    | 0.7919          |
| m28    | 4 (butyl)                 | NCCCC         | 207.27 | 2.225 | 5    | 0.7518          |
| m17    | 5 (pentyl)                | NCCCCC        | 221.30 | 2.615 | 6    | 0.7498          |
| m22    | 6 (hexyl)                 | NCCCCCC       | 235.33 | 3.005 | 7    | 0.7377          |
| **m05**| **11 (undecyl)**          | **NCCCCCCCCCCC** | **305.46** | **4.956** | **12** | **?**       |

## Observed trend
- QED **rises** from n=2 (0.7621) to n=3 (0.7919) — likely because ALogP moves from sub-optimal (~1.4) toward the optimal range (~1.8).
- QED then **declines** from n=3 onward as ALogP, MW, and RotB grow beyond drug-like ranges.
- Changes per added CH2: +0.030, -0.040, -0.002, -0.012.

## Model: Quadratic fit on all 5 measured points

Fitted QED = a*n^2 + b*n + c via least-squares (n = chain carbon count):

**QED(n) = -0.003264*n^2 + 0.017024*n + 0.74932**

Goodness of fit:
| n | Predicted | Measured | Error   |
|---|-----------|----------|---------|
| 2 | 0.7703    | 0.7621   | +0.008  |
| 3 | 0.7710    | 0.7919   | -0.021  |
| 4 | 0.7652    | 0.7518   | +0.013  |
| 5 | 0.7528    | 0.7498   | +0.003  |
| 6 | 0.7340    | 0.7377   | -0.004  |

RMSE ~ 0.012. Max absolute error ~ 0.021 (at n=3).

### Why quadratic?
- Captures the non-monotonic shape (rise then fall).
- The downward-opening parabola (a < 0) naturally models accelerating decline at longer chains — physically expected because ALogP (~5.0), MW (~305), and RotB (12) at n=11 all push well outside drug-like ranges, compounding QED penalties.
- The vertex is at n ~ 2.6, consistent with the observed peak near n=3.

### Why not linear or higher-order?
- A linear fit ignores the curvature and the task instruction to fit the observed non-linear trend.
- A cubic on these 5 points extrapolates to QED > 1 at n=11 (nonsense), confirming that higher-order polynomials are unreliable for extrapolation with so few points.

## Extrapolation to n = 11

QED(11) = -0.003264 * 121 + 0.017024 * 11 + 0.74932
        = -0.39494 + 0.18726 + 0.74932
        = **0.5416**

Cross-check via ALogP quadratic (same data reparametrised): QED(4.956) ~ 0.535 — consistent.

## Final prediction

**QED(m05) ~ 0.54**

Congeners found: m15, m24, m28, m17, m22
