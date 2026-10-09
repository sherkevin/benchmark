# Task notes

## Pool analysis

33 amide molecules `R-C(=O)N-alkyl`. Scaffold `R` defines the series; N-alkyl chain
length varies within a series. Four series identified by scaffold:

- **Series A — methoxy-benzoyl** `COc1ccc(cc1)C(=O)N` (psa 38.33) — the m05 TARGET series:
  - m15 (chain 2), m24 (c3), m28 (c4), m17 (c5), m22 (c6) — all SHORT chain.
  - m05 (c11) is the unsynthesized target.
- **Series B — nitro-benzoyl** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24):
  - m09 (c2), m02 (c3), m12 (c4), m32 (c5), m33 (c6), m29 (c7), m31 (c8), m06 (c9), m10 (c10).
- **Series C — polyol** `OCC(O)C(O)C(O)C(=O)N` (psa 110.02):
  - m14 (c2), m27 (c3), m01 (c4), m19 (c5), m03 (c6), m21 (c7), m18 (c8), m07 (c9), m25 (c10).
- **Series D — biphenyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.1): excluded (psa<40).

## Job 1 — predict m05 (Series A, chain 11)

m05 is never synthesized → extrapolate from the short-chain Series A members
m15/m24/m28/m17/m22 (chains 2–6). Measured QED:

| mol | chain | QED |
|-----|-------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |  ← peak
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

QED rises to a peak at chain 3 then declines — clearly curved, not linear, so I
fit a degree-2 polynomial (not a line, not the nearest point).

`fit_trend(xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377], x_query=11, degree=2)`:
- prediction at chain 11 = **0.5416**
- coeffs (low→high) = [0.74932, 0.017024, −0.003264]
- RMSE on the 5 points = 0.01188

This is an extrapolation beyond the measured range (chains 2–6 → 11); the downward
parabolic trend projects the long-chain analog well below the measured values, near
0.54. Target QED ≈ 0.5416.

## Job 2 — backup lead (QED≥0.6, psa≥40, not Series A)

screen_candidates() confirms psa≥40 passes = Series B (72.24) ∪ Series C (110.02).
Series A excluded (target series); Series D excluded (psa 29.1).

Measured Series C (polyol) — all far below 0.6, dead end:
- m14 (c2) = 0.3237
- m27 (c3) = 0.333

Measured Series B (nitro):
- m09 (c2) = 0.5826  (just under)
- **m02 (c3) = 0.6046**  ✓ passes QED gate
- m12 (c4) = 0.4709
- m32 (c5) = 0.4683

The nitro series peaks at chain 3. m02 is the only measured molecule meeting all
three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. nitro-benzoyl scaffold ≠ m05's methoxy-benzoyl (not same series) ✓

**Lead = m02, measured QED = 0.6046.**

## Budget

12 total, used 11: 5 (Series A: m15,m24,m28,m17,m22) + 1 (m09) + 1 (m02) + 2
(Series C: m14,m27) + 2 (m12,m32). Kept 1 in reserve.

## Deliverables

- target_qed = 0.5416 (degree-2 extrapolation of Series A to chain 11)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids = [m15, m24, m28, m17, m22] (the short-chain pool members of m05's
  series; m05 itself is the unsynthesized target, not a measured pool congener)
