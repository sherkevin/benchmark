# Predicting QED of `m05` (unmeasurable target)

## Target
- `m05`: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold = 4-methoxybenzamide; N-alkyl chain of **11 carbons** (`N` + 11×`C`).
- `measure_qed(m05)` → `probe_unavailable` (never synthesized). Must extrapolate.

## Step 1 — find the congeneric series
Searched the pool with the scaffold substring `COc1ccc(cc1)C(=O)N` (the methoxybenzamide
core; only the N-alkyl tail differs between members). 2 search pages, 6 matches total =
the target plus 5 short-chain congeners:

| mol_id | tail SMILES           | chain N | mw     | alogp  | psa    | rotb | arom |
|--------|-----------------------|:------:|--------|--------|--------|:----:|:----:|
| m15    | `NCC`                 |   2    | 179.22 | 1.445  | 38.33  |  3   |  1   |
| m24    | `NCCC`                |   3    | 193.25 | 1.835  | 38.33  |  4   |  1   |
| m28    | `NCCCC`               |   4    | 207.27 | 2.225  | 38.33  |  5   |  1   |
| m17    | `NCCCCC`              |   5    | 221.30 | 2.615  | 38.33  |  6   |  1   |
| m22    | `NCCCCCC`             |   6    | 235.33 | 3.005  | 38.33  |  7   |  1   |
| m05    | `NCCCCCCCCCCC`        |  11    | 305.46 | 4.956  | 38.33  |  12  |  1   | (target)

The five measurable congeners cover only N = 2–6; the target sits at N = 11, well outside
the measured range → this is an **extrapolation** problem. Descriptors mw, alogp, rotb all
grow linearly with N (confirming one series); psa and arom are constant, as expected for a
single scaffold.

## Step 2 — measure the series (5 budget units)
`measure_qed` on each congener (all real calls):

| N | mol_id | QED    |
|:-:|:------:|-------:|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Budget used: 2 search + 5 measure = 7 of 12.

## Step 3 — the trend
QED rises from N=2 to a **peak at N=3** (0.7919), then declines monotonically through
N=6. The rise-then-fall shape rules out a straight line (the task warns against assuming
one). The natural model for a peaked trend is a downward-opening **quadratic** in N:

    QED(N) = a·N² + b·N + c

Least-squares fit over the 5 points (verified by hand via centered normal equations,
u = N−4; Σu=0, Σu²=10, Σu⁴=34, ȳ=0.75866, Σuy=−0.0909, Σu²y=7.5409):

    a = −0.003257,  b = +0.016967,  c = +0.749421

Vertex at N* = −b/(2a) ≈ 2.61 — consistent with the observed peak at N=3.

In-range residuals (predicted − observed):
  N=2: +0.0082,  N=3: −0.0209,  N=4: +0.0134,  N=5: +0.0030,  N=6: −0.0038
SSR ≈ 7.1e-4,  max |residual| ≈ 0.021 (the parabola slightly undershoots the sharp N=3
peak). For comparison, a straight line through the same points gives SSR ≈ 8.6e-4 and
systematically misses the peak, so the quadratic is the better description of both level
and shape. A cubic (4 params vs 5 pts, 1 d.o.f.) would chase the single peak and risk a
wild long-range extrapolation — deliberately not used.

## Step 4 — extrapolate to N = 11
Using the quadratic fit:

    QED(11) = −0.003257·121 + 0.016967·11 + 0.749421
            = −0.3941 + 0.1866 + 0.7494
            ≈ 0.542

Sanity checks:
- It is below the last measured value (N=6 → 0.738), as required: ALogP, MW and rotatable
  bonds all grow with N, so every chain-length-sensitive QED penalty worsens from N=6 to
  N=11 — QED must fall, and it does.
- The parabola accelerates downward past its vertex, so the drop per carbon steepens
  outside the measured range (≈ −0.039/C over N=6→11 vs ≈ −0.018/C observed in N=3→6).
  That acceleration is physically plausible: at N=11 the molecule enters stiff QED
  penalty zones (ALogP 4.96 ≫ 3, rotb 12, MW 305), where the ALogP and rotatable-bond
  desirability functions curve downward, compounding the decline.
- A naive straight-line projection of the N=4→6 decline would give ≈ 0.70; it ignores
  the curvature the data already show (peak at N=3, downward parabola) and is the model
  the task explicitly tells us not to use. The quadratic is the defensible fit.

## Answer
- **Predicted QED of `m05`: ≈ 0.542**
- Congeners in the same series (all found via the scaffold search): m15, m24, m28, m17, m22.
