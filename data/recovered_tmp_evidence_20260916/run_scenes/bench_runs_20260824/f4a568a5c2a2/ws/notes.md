# Notes: Predict QED of m05

## Target
- `m05` SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: 4-methoxybenzoyl amide (p-anisamide) with an N-alkyl chain.
- Chain = N + 11 CH2 → the **C11** member. rotb=12 confirms: each CH2 adds one
  rotatable bond, so rotb = n+1 → n=11. ✓

## Search (budget: 12 shared)
- Query `COc1ccc(cc1)C(=O)N` (scaffold substring of m05) → 6 matches over 2 pages.
- All 6 share that scaffold and differ only in N-alkyl length → SAME congeneric series.
- Budget used: 2 (search pages) + 5 (measurements) = 7 of 12.

## Congeners (same series as m05)
| mol_id | tail       | n  | rotb | mw     | alogp  | measured QED |
|--------|------------|----|------|--------|--------|--------------|
| m15    | NCC        | 2  | 3    | 179.22 | 1.445  | 0.7621       |
| m24    | NCCC       | 3  | 4    | 193.25 | 1.835  | 0.7919       |
| m28    | NCCCC      | 4  | 5    | 207.27 | 2.225  | 0.7518       |
| m17    | NCCCCC     | 5  | 6    | 221.30 | 2.615  | 0.7498       |
| m22    | NCCCCCC    | 6  | 7    | 235.33 | 3.005  | 0.7377       |
| m05    | N…(11C)    | 11 | 12   | 305.46 | 4.956  | — (target)   |

rotb = n+1 for every member → confirms m05 is the C11 extrapolation point.

## Observed trend (measured n=2..6; predict n=11 — extrapolation beyond data)
- Rises n=2→3 (0.7621→0.7919), **peak at n=3**, then declines monotonically:
  0.7518 → 0.7498 → 0.7377. Clear **non-linearity** (a peak + curvature).
- Step drops on the tail: −0.040, −0.002, −0.012 (irregular; last > middle).
- At n=11, ALogP=4.956 (>3), ROTB=12 (>7), MW=305 (>300) — all in penalty zones,
  so QED at n=11 must be well below the last measured 0.7377.

## Fits
**Quadratic (fit to all 5 points, centered u = n−4):**
y = 0.76519 − 0.00909·u − 0.003264·u²   (least squares; Σu=Σu³=0 ⇒ decoupled)
- Vertex (max) at u≈−1.39 ⇒ n≈2.6 — matches observed peak near n=3. Good fit.
- Residuals: n=6 predicted 0.7340 vs actual 0.7377; peak n=3 predicted 0.7710 vs
  actual 0.7919 (slightly under, acceptable).
- At n=11 (u=7): 0.76519 − 0.0636 − 0.1599 = **0.5416 ≈ 0.542**.

**Linear on the declining tail (n=3..6), as a no-curvature upper reference:**
slope −0.0165/CH2, intercept 0.8319 → at n=11: **0.651**.
Linear ignores the peak/curvature the task warns against; serves as an upper bound.

## Prediction
- The task says fit the observed (curved) trend, not assume linear. The quadratic IS
  that fit → central estimate **0.542**.
- Caveat: a downward parabola accelerates without bound, but QED is a bounded [0,1]
  score whose penalty functions saturate, so pure parabolic extrapolation (5 steps
  beyond the data, n=6→11) tends to overshoot downward → small upward hedge.
- Net: the true value sits between the quadratic (0.542) and the linear (0.651);
  honoring curvature, lean to the lower end with a modest saturation hedge.
- **Predicted QED for m05 ≈ 0.55.**
  Consistency check: from n=6 (0.7377) to n=11 that's −0.19 over 5 steps ≈ −0.038/step,
  ~2.3× the n=3→6 rate — consistent with ALogP/ROTB/MW all entering penalty zones.

## Answer
- target_qed = 0.55
- congener_ids (all pool molecules in m05's series, incl. the unmeasurable target):
  m05, m15, m17, m22, m24, m28
