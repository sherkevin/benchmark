# Notes — QED prediction task

## Setup
- Budget: 12 measure_qed calls total, shared by Task 1 & Task 2.
- Task 1: predict QED of m05 (anisamide scaffold, chain_len 11, NOT synthesized).
  Predict from congeneric series trend (same scaffold, varying N-alkyl chain length).
- Task 2: pick ONE molecule with (a) measured QED >= 0.6, (b) psa >= 40,
  (c) NOT in m05's congeneric series (anisamide).

## Scaffolds (from group_by_scaffold)
4 series:
1. anisamide `COc1ccc(cc1)C(=O)N` — psa=38.33 (all). members:
   m15(c2), m24(c3), m28(c4), m17(c5), m22(c6), m05(c11, TARGET).
   Only m05 unmeasurable. psa 38.33 < 40 so fails Task2 constraint2 anyway, and
   it's m05's own series (fails constraint 3). Exclude from Task 2.
2. nitro `O=[N+]([O-])c1ccc(cc1)C(=O)N` — psa=72.24 (all). members:
   m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10).
   psa >= 40, not anisamide => Task 2 eligible.
3. glycerol `OCC(O)C(O)C(O)C(=O)N` — psa=110.02 (all). members:
   m14(c2), m27(c3), m01(c4), m19(c5), m03(c6), m21(c7), m18(c8), m07(c9), m25(c10).
   psa >= 40, not anisamide => Task 2 eligible.
4. biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — psa=29.1 (all) < 40 => excluded from Task 2.

## Task 2 candidate pools (psa>=40, not anisamide): nitro series & glycerol series.

## Strategy
- Task 1: measure all 5 measurable anisamide members (m15,m24,m28,m17,m22),
  fit QED vs chain_len, evaluate at chain_len=11. => 5 budget.
- Task 2: measure a good nitro candidate first (m32: ALOGP 2.515, ROTB 6, MW 236,
  well-balanced). Use qed_components (free) to see if nitro triggers an ALERT penalty.
  If m32 >= 0.6 => done. Else adjust (try m33 / glycerol).

## Measurements (budget 12; used 10)

### Task 1 — anisamide series (m05 = chain_len 11, not synthesized)
| chain | mol | QED |
|---|---|---|
| 2  | m15 | 0.7621 |
| 3  | m24 | 0.7919 |  <- peak
| 4  | m28 | 0.7518 |
| 5  | m17 | 0.7498 |
| 6  | m22 | 0.7377 |
| 11 | m05 | PREDICT (not measurable)

Fit results for x_query=11:
- quad through all 5 points (c2..c6): 0.5416, rmse 0.0119
  coeffs [0.74932, 0.017024, -0.003264]  => shallow concave-down; long extrapolation bends down.
- linear through c3..c6 (exclude c2 outlier): 0.6508, rmse 0.0088
- quad through c3..c6: 0.9378, rmse 0.0054 (clearly overfits 4 pts, unphysical upward curl)
- linear through all 5: 0.695, rmse 0.0131

Decision: The c2 point (0.7621) is a dip below the c3 peak (0.7919); the genuine trend
from the peak onward is a gentle monotonic decline. A quadratic through all 5 points
bends downward too steeply at x=11 because it must dip to fit c2 then rise to c3.
Extrapolation is inherently uncertain, but the most defensible single number is the
quadratic through ALL 5 measured points of the congeneric series (uses all data,
smallest degree that captures the curvature), = 0.5416. I report target_qed = 0.5416
(~0.54). Note honest uncertainty band ~0.54–0.70 across reasonable fits.

### Task 2 — need psa>=40, QED>=0.6, NOT anisamide.
Candidate series with psa>=40: nitro (72.24) and glycerol (110.02).
- m32 (nitro, c5): 0.4683. components: ALERTS=0.0817 (nitro alert cripples it).
- m33 (nitro, c6): 0.4591. components: ALERTS=0.0817 (same). => whole nitro series capped.
- m09 (nitro, c2, shortest/best): 0.5826. STILL < 0.6. Best nitro is a near-miss.
- m19 (glycerol, c5): 0.3244. components HBD=0.0555 (pentahydroxy => HBD floor ~0),
  AROM=0.4725, ALOGP low. => glycerol series capped low.
- m21 (glycerol, c7): 0.3297. same HBD floor.

Conclusion: NO molecule in the pool simultaneously satisfies psa>=40, QED>=0.6, and
not-anisamide. The two psa>=40 series are both structurally capped (nitro via ALERTS,
glycerol via HBD) below 0.6. The biphenyl series has QED likely >=0.6 (low rotb, no
alerts expected) but psa=29.1 < 40, so it fails constraint 2.

### Resolution for Task 2
The constraints as written cannot all be met by any pool molecule. The two psa>=40
series are both structurally capped below 0.6 (nitro via ALERTS, glycerol via HBD=0.0555
floor). The biphenyl series would score well on QED but psa=29.1 < 40 (excluded).

Best-of-the-rest: m09 (nitro, c2, psa 72.24>=40, not anisamide, measured QED 0.5826) —
highest QED of any non-anisamide psa>=40 molecule; misses the >=0.6 cut narrowly.
components(m09): MW 0.4117, ALOGP 0.8537, HBA 0.9848, HBD 0.9866, PSA 0.9302,
ROTB 0.9927, AROM 0.8273, ALERTS 0.2414. ALERTS still caps the nitro series below 0.6
even at the shortest chain.

I report m09 as the lead (lead_mol_id=m09, lead_qed_measured=0.5826) while being
explicit that NO pool molecule satisfies all three constraints simultaneously; m09
fails only the >=0.6 cut and is the closest achievable.
