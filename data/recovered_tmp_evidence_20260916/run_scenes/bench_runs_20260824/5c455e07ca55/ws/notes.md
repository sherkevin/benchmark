# Notes

## Budget
12 measurements total, shared by both jobs. Used all 12.

## Congeneric series (by scaffold, only N-alkyl chain length differs)

Reading SMILES, the pool falls into 4 amide series:

1. **Methoxy-phenyl amide** `COc1ccc(cc1)C(=O)N-` — psa 38.33
   - m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6), **m05 (C11, the target)**
2. **Nitro-phenyl amide** `O=[N+]([O-])c1ccc(cc1)C(=O)N-` — psa 72.24
   - m09 (C2), m02 (C3), m12 (C4), m32 (C5), m33 (C6), m29 (C7), m31 (C8), m06 (C9), m10 (C10)
3. **Biphenyl amide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` — psa 29.1 (< 40, fails lead screen)
   - m13, m11, m04, m30, m23, m20, m26, m08, m16
4. **Sugar (polyol) amide** `OCC(O)C(O)C(O)C(=O)N-` — psa 110.02
   - m14 (C2), m27 (C3), m01 (C4), m19 (C5), m03 (C6), m21 (C7), m18 (C8), m07 (C9), m25 (C10)

m05's series (methoxy) = {m05, m15, m24, m28, m17, m22}. The pool holds short chains C2–C6; m05 is the long-chain (C11) target, never synthesized.

## Job 1 — predict QED of m05 (methoxy, C11)

Measured the 5 short-chain methoxy congeners:

| chain | mol_id | QED    |
|-------|--------|--------|
| C2    | m15    | 0.7621 |
| C3    | m24    | 0.7919 |  ← peak
| C4    | m28    | 0.7518 |
| C5    | m17    | 0.7498 |
| C6    | m22    | 0.7377 |

Trend: rises C2→C3, then declines. Not a straight line — there is a clear peak/curvature,
so a degree-2 (parabolic) fit is the right shape.

Fit (degree 2, xs=[2,3,4,5,6], ys above), evaluate at x=11 (extrapolation beyond C6):
- coefficients (low→high): [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188
- **prediction at C11 = 0.5416**

(Sanity comparison — degree-1 linear gives 0.695, but it ignores the peak at C3 and
flattens the curvature; degree-2 is the better description of the observed trend, so I
report 0.5416. Either way the extrapolation continues the decline seen from C3 onward.)

**target_qed = 0.5416**

## Job 2 — pick a backup lead (QED≥0.6 AND psa≥40 AND not methoxy series)

`screen_candidates()` returned the 18 molecules with psa≥40. Apply constraint 3
(discard m05's methoxy series) and constraint 1 must come from a real measurement.

Elimination:
- Biphenyl series: psa 29.1 < 40 → all fail screen. Discard.
- Methoxy series: excluded by constraint 3 (same series as m05).
- Remaining candidates: nitro series (psa 72.24) + sugar/polyol series (psa 110.02).

Measurements used for the lead search (budget was tight; picked adaptively):

Nitro series (looking for a peak; methoxy peaked at C3, so nitro plausibly too):
- m09 (C2) = 0.5826
- m02 (C3) = **0.6046**  ← ≥ 0.6, the peak
- m12 (C4) = 0.4709
- m32 (C5) = 0.4683
→ Nitro peaks at C3 then declines; only m02 reaches ≥0.6.

Sugar/polyol series (very negative alogp, 4 HBDs — QED penalizes):
- m14 (C2) = 0.3237
- m03 (C6) = 0.3282
- m25 (C10) = 0.3148
→ Flat and low; none near 0.6.

### Lead: m02
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓ (free, from pool.tsv)
- scaffold = nitro-phenyl amide ≠ methoxy-phenyl amide of m05 ✓ (not in m05's series)

All three constraints satisfied. **lead_mol_id = m02, lead_qed_measured = 0.6046.**

## Final answer
- target_qed = 0.5416
- lead_mol_id = "m02"
- lead_qed_measured = 0.6046
- congener_ids = ["m05","m15","m24","m28","m17","m22"]  (methoxy series, the same series as m05)
