# Notes

## Setup
- Pool: 33 molecules in 4 congeneric series (by scaffold). Each series varies only N-alkyl chain length.
- Budget: 12 measurements total, shared by Task 1 & Task 2.
- m05 (COc scaffold, chain 11) is unsynthesized → unmeasurable; must predict from its series.

## Scaffold groups (from group_by_scaffold)
1. **COc** `COc1ccc(cc1)C(=O)N` psa=38.33: m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target)**.
2. **nitro** `O=[N+]([O-])c1ccc(cc1)C(=O)N` psa=72.24: m09(2),m02(3),m12(4),m32(5),m33(6),m29(7),m31(8),m06(9),m10(10).
3. **sugar** `OCC(O)C(O)C(O)C(=O)N` psa=110.02: m14(2),m27(3),m01(4),m19(5),m03(6),m21(7),m18(8),m07(9),m25(10).
4. **biphenyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` psa=29.1: m13..m16.

## Task 1 — m05 (COc, chain 11) QED prediction
Measured COc congeners (chain → QED):
- c2 m15=0.7621, c3 m24=0.7919, c4 m28=0.7518, c5 m17=0.7498, c6 m22=0.7377.
- Observed: peak at c3 (0.792), then gentle decline c3→c6 of ~−0.018/step (Δ: −0.040, −0.002, −0.012).
- fit_trend to x=11 (5 steps beyond c6):
  - linear (deg 1): **0.695** (slope −0.00909/step, RMSE 0.0131)
  - quadratic (deg 2): 0.542 (RMSE 0.0119, concave-down, projects accelerated −0.039/step drop)
- Choice: **0.695** (linear). The c3→c6 slope is gentle (~−0.018/step); linear's −0.009/step is a
  modest underestimate of recent decline, while quadratic's −0.039/step over-extrapolates the single
  c3→c4 step into an accelerating penalty. Linear is the more reliable steady-decay predictor. The
  true value likely sits in the 0.54–0.70 band; 0.695 is the best single point estimate.

## Task 2 — lead molecule (QED≥0.6 AND psa≥0.6 AND psa≥40 AND not in COc series)
- COc series: excluded (constraint 3; also psa 38.33 < 40).
- biphenyl series: excluded (psa 29.1 < 40).
- **nitro series** (psa 72.24 ✓): m09(c2)=0.5826, **m02(c3)=0.6046 ✓**, m32(c5)=0.4683, m33(c6)=0.4591.
  Trend peaks at short chain then declines (nitro is a structural alert); m02 at c3 is the maximum
  and the only nitro member ≥ 0.6.
- **sugar series** (psa 110.02 ✓): m03(c6)=0.3282, m21(c7)=0.3297 — far below 0.6 (excessive
  polarity / 5 H-bond donors cap the series). Dead end.
- Winner: **m02** — QED 0.6046 ≥ 0.6 ✓, psa 72.24 ≥ 40 ✓, not in COc series ✓.

## Budget ledger (12 total)
Round 1 (8): m15, m24, m28, m17, m22 (Task1 series); m32, m33, m09 (Task2 nitro probes).
Round 2 (3): m02 (Task2 nitro c3 — winner), m03 (Task2 sugar c6), m21 (Task2 sugar c7).
Total used: 11/12. (1 reserved, unused.)
