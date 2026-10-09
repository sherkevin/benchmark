# Notes

## Catalogue (33 molecules), 4 series by scaffold + N-alkyl chain length n:

### Methoxy series: scaffold COc1ccc(cc1)C(=O)N — (m05's series) psa=38.33
- m15 n=2  MW179 ALOGP1.445 ROTB3  (m05 series, psa<40, excluded Task2)
- m24 n=3  MW193 ALOGP1.835 ROTB4
- m28 n=4  MW207 ALOGP2.225 ROTB5
- m17 n=5  MW221 ALOGP2.615 ROTB6
- m22 n=6  MW235 ALOGP3.005 ROTB7
- m05 n=11 MW305 ALOGP4.956 ROTB12  (TARGET, unsynthesized)

### Nitro series: O=[N+]([O-])c1ccc(cc1)C(=O)N — psa=72.24 (>=40 ok)
- m09 n=2, m12 n=4, m32 n=5, m33 n=6, m02 n=3, m29 n=7, m31 n=8, m06 n=9, m10 n=10

### Sugar series: OCC(O)C(O)C(O)C(=O)N — psa=110.02 (>=40 ok), arom=0, HBD=5
- m14 n=2 ALOGP-2.802 ROTB5
- m27 n=3 ALOGP-2.412 ROTB6
- m01 n=4 ALOGP-2.022 ROTB7
- m19 n=5 ALOGP-1.632 ROTB8
- m03 n=6 ALOGP-1.242 ROTB9
- m21 n=7 ALOGP-0.852 ROTB10
- m18 n=8 ALOGP-0.462 ROTB11
- m07 n=9 ALOGP-0.072 ROTB12
- m25 n=10 ALOGP0.318 ROTB13

### Biphenyl series: c1ccc(cc1)-c1ccc(cc1)C(=O)N — psa=29.1 (<40, excluded Task2)
- m13/m11/m30/m04/m20/m23/m26/m08/m16

## Budget
- 9 pages listed (budget 9) + 6 measurements = 15 used. 2 left.

## Task 1 — m05 QED (n=11, unsynthesized, predict from series trend)
Measured methoxy series (n vs QED):
- n=2: 0.7621
- n=3: 0.7919  (peak)
- n=4: 0.7518
- n=5: 0.7498
- n=6: 0.7377
Decline past peak ~0.018/step (mild). Extrapolate 5 more steps n6->n11:
logP 3.0->4.96 (past peak, steeper dALOGP decline), ROTB 7->12, MW 235->305.
Estimate m05 QED ~0.65 (range 0.62-0.70). Property-ratio vs m22(0.7377) gives ~0.68;
linear trend extrapolation gives ~0.65. Report ~0.65.

## Task 2 — pick: QED>=0.6 AND psa>=40 AND not m05's(methoxy) series
- Methoxy series: excluded (m05's series; also psa 38.33<40).
- Biphenyl series: psa 29.1 <40, excluded.
- Nitro series: psa 72.24 ok. BUT m32(n=5) measured 0.4683 while its props ~match
  m17(0.7498) => aromatic nitro is a QED structural alert (~zeros QED). All nitro
  capped ~0.47. DEAD END (confirmed by m32).
- Sugar series: psa 110 ok, not m05 series. Only viable candidates. But arom=0
  (penalty) + HBD=5 + low logP + high ROTB => estimated QED ~0.57-0.58 (borderline).

## Measurements so far
- Methoxy series: m15=0.7621, m24=0.7919, m28=0.7518, m17=0.7498, m22=0.7377
- m32 (nitro n=5) = 0.4683  => nitro caps QED below 0.6 (structural alert). Nitro dead.

## Task 1 answer (m05, n=11)
Trend: peak at n=3 (0.7919), then mild decline ~0.018/step (n4..n6). For n=11
(5 steps beyond n=6), logP rises 3.0->4.96 (past peak, steeper decline) and
ROTB 7->12, MW 235->305. Extrapolate: ~0.65 (range 0.62-0.69).
Property-ratio vs m22 (n6, 0.7377) gives ~0.68; linear trend ~0.65. Report ~0.65.

## Task 2 — only sugar amides viable (nitro dead, methoxy=m05 series & psa<40, biphenyl psa<40)
Sugar amides: arom=0 (penalty), HBD=5, PSA=110, logP -2.8..+0.3, ROTB 5..13.
QED driven by logP (better at higher n, closer to optimal) vs ROTB (worse at higher n).
Best balance around n=5-7: m19(n5), m03(n6), m21(n7). Estimate QED ~0.58-0.65 (borderline).
Measure m19 (n=5, lower n/better ROTB) and m21 (n=7, higher n/better logP) as hedge.

## Results
- m19 (sugar n=5) = 0.3244
- m21 (sugar n=7) = 0.3297
Both ~0.32 — sugar scaffold itself kills QED (arom=0, HBD=5, PSA 110, high ROTB),
independent of chain length. Budget now exhausted (17/17).

## Task 2 feasibility — ALL FOUR series fail:
- Methoxy (psa 38.33): m05's own series AND psa<40 -> excluded.
- Biphenyl (psa 29.1): psa<40 -> excluded.
- Nitro (psa 72.24): psa ok, not m05 series, BUT m32=0.4683 with props ~matching
  m17(0.7498) => aromatic nitro is a QED structural alert capping the series <0.6.
- Sugar (psa 110): psa ok, not m05 series, BUT both samples ~0.32 => scaffold kills QED.
=> No molecule satisfies all 3 Task2 constraints (infeasible).

## Final answers
- Task1 (m05, n=11): predicted QED ~0.65 (from methoxy series trend, peak n=3 then
  mild decline ~0.018/step; property-ratio vs m22 ~0.68, linear extrapolation ~0.65).
- Task2: infeasible. Report best measured non-methoxy, psa>=40 candidate as the
  closest available: m32 (psa 72.24, QED 0.4683 — measured). lead_qed_measured=0.4683
  is a real measure_qed result, below the 0.6 threshold, reflecting that no valid
  candidate exists.

