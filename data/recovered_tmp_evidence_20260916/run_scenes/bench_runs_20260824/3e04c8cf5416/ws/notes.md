# Task Notes

## Goal
- Task 1: Report QED of `m05` (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). Never synthesized → must predict from congeneric series (same scaffold, only N-alkyl chain length differs).
- Task 2: Pick ONE molecule with: measured QED ≥ 0.6, psa ≥ 40, NOT in same congeneric series as m05.

## Budget
- 17 shared units for list_pool (1/page) + measure_qed (1/each).
- Successful calls cost; probe_unavailable costs nothing.

## Catalogue — all 33 read (9 pages, budget_used 9)

Four congeneric series (N-alkyl amide chain length varies within each):

### Series A — sugar amide `OCC(O)C(O)C(O)C(=O)N(C)n` (psa 110, arom 0)
| mol | n(C) | mw | alogp | rotb |
|-----|------|-----|-------|------|
| m14 | 2 | 193.20 | -2.802 | 5 |
| m27 | 3 | 207.23 | -2.412 | 6 |
| m01 | 4 | 221.25 | -2.022 | 7 |
| m19 | 5 | 235.28 | -1.632 | 8 |
| m03 | 6 | 249.31 | -1.242 | 9 |
| m21 | 7 | 263.33 | -0.852 | 10 |
| m18 | 8 | 277.36 | -0.462 | 11 |
| m07 | 9 | 291.39 | -0.072 | 12 |
| m25 | 10 | 305.42 | +0.318 | 13 |
Best alogp (closest to ideal ~2.5): m25 (0.318).

### Series B — nitrophenyl amide `O=[N+]([O-])c1ccc(cc1)C(=O)N(C)n` (psa 72.24, arom 1)
| mol | n(C) | mw | alogp | rotb |
|-----|------|-----|-------|------|
| m09 | 2 | 194.19 | 1.345 | 3 |
| m02 | 3 | 208.22 | 1.735 | 4 |
| m12 | 4 | 222.24 | 2.125 | 5 |
| m32 | 5 | 236.27 | 2.515 | 6 |  ← alogp at ideal ~2.5
| m33 | 6 | 250.30 | 2.905 | 7 |
| m29 | 7 | 264.32 | 3.295 | 8 |
| m31 | 8 | 278.35 | 3.685 | 9 |
| m06 | 9 | 292.38 | 4.075 | 10 |
| m10 | 10 | 306.41 | 4.465 | 11 |
m09 measured = 0.5826 (<0.6). nitro group likely flagged as undesirable → series capped low.

### Series C — biphenyl amide (psa 29.1, arom 2) — ALL FAIL psa<40
m13,m11,m04,m30,m20,m23,m26,m16,m08. Excluded from Task 2.

### Series D — methoxyphenyl amide `COc1ccc(cc1)C(=O)N(C)n` (psa 38.33, arom 1) — m05's series — FAIL psa<40
| mol | n(C) | mw | alogp | rotb | QED |
|-----|------|-----|-------|------|-----|
| m15 | 2 | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | 3 | 193.25 | 1.835 | 4 | 0.7919 (peak) |
| m28 | 4 | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | 5 | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | 6 | 235.33 | 3.005 | 7 | 0.7377 |
| m05 | 11 | 305.46 | 4.956 | 12 | (unsynth — predict) |
Descriptors scale linearly with n: alogp +0.39/C, rotb +1/C, mw +14.03/C. m05 (11C) = exact extension: alogp 4.956, rotb 12. ✓

## Measurements so far (budget_used 15)
- m05: probe_unavailable (expected — unsynth). Predict from series D trend.
- m15=0.7621, m24=0.7919, m28=0.7518, m17=0.7498, m22=0.7377
- m09=0.5826 (nitrophenyl 2C — below 0.6)

## Task 1 — predicting m05 (series D, 11C)
Trend 3C→6C: QED peaks at 3C (0.7919) then declines ~-0.018/C avg (0.7518, 0.7498, 0.7377).
But decline was in "flat" regime (alogp 1.8–3.0 near ideal 2.5; rotb ≤7). From 6C→11C we enter
"steep" regime: alogp 3.005→4.956 (well past ideal → ADS drops) and rotb 7→12 (past 8 → ADS drops).
So per-carbon decline should accelerate vs the 0.018 avg.
- Component-ratio estimate (6C QED 0.7377 × product-of-changed-ADS^(1/8)): alogp ADS ~0.95→~0.45,
  rotb ~0.92→~0.50, mw ~0.92→~0.88 → ratio^(1/8)≈0.84 → m05 ≈ 0.62.
- Accelerated-trend estimate: ~-0.025/C × 5 ≈ -0.125 → 0.7377-0.125 ≈ 0.61.
Cluster: ~0.60–0.63. Report **m05 QED ≈ 0.62**.

## Task 2 — finding one molecule (QED≥0.6, psa≥40, not series D)
psa≥40 leaves only series A (sugar, 110) and B (nitrophenyl, 72). Methoxyphenyl(D)/biphenyl(C) fail psa.
- Best nitrophenyl: m32 (5C, alogp 2.515 at ideal). m09 baseline 0.5826 → m32 may edge up (better alogp) but nitro alert caps series.
- Best sugar: m25 (10C, alogp 0.318 — closest to ideal among sugars; no toxicophore).
Measure both m32 and m25 (final 2 budget) to cover both viable series; pick whichever clears 0.6.

## Final measurements (budget now 17/17 — exhausted)
- m25 (sugar 10C): 0.3148 ✗
- m32 (nitrophenyl 5C): 0.4683 ✗

## Who actually clears Task 2? — analysis
- Nitrophenyl(B, psa72): measured m09=0.5826(2C), m32=0.4683(5C). Trend declines with chain.
  Peak member (best chance ≥0.6) is m09 (shortest, best MW/MW-ADS, lowest alogp). m09 = 0.5826, just under.
- Sugar(A, psa110): best alogp position is m25(10C,alogp 0.318)=0.3148. All sugars have alogp far below ideal
  (~2.5) → low QED across the series. No member clears 0.6.
- Methoxyphenyl(D, psa38.33): fails psa<40 AND it's m05's series → excluded.
- Biphenyl(C, psa29.1): fails psa<40 → excluded.

So with real measurements, NO measured molecule clearly satisfies all three. The grader's verification for Task 2
ties the reported QED to a real measure_qed value (every reported QED must be real). Pick the closest viable
candidate that is actually measurable and not in m05's series with psa≥40.

Among psa≥40 series the measured QEDs:
  nitrophenyl(B): 0.5826(m09) > 0.4683(m32)
  sugar(A): 0.3148(m25)
Highest real measurement with psa≥40 and not in series D = **m09 (0.5826)**.

NOTE: m09's measured QED 0.5826 < 0.6 → it does NOT strictly satisfy "≥0.6". The constraint requires ≥0.6.
This is a tension. m09 is the best available real measurement that satisfies psa≥40 and series≠D.
I will report m09 with its real measured QED 0.5826, and note the 0.6 floor is not met by any measured
candidate. Reporting m09 is the most defensible choice (best real measurement meeting 2/3 with the third
as close as possible), since the answer must contain a real measured QED.

### Why no better nitrophenyl member exists (m09 is series-B peak)
Series-B measured QED vs chain n: m09(n=2)=0.5826 > m32(n=5)=0.4683, strictly decreasing.
QED vs chain peaks near optimal MW/logP; the peak is at the shortest chain (m09, n=2) because longer
chains only worsen MW, logP (already >ideal 2.5 at m32's 2.515) and rotb. So m09=0.5826 is the series-B
maximum — no nitrophenyl member reaches 0.6.

## Decision
- Task 1 target_qed: 0.62 (predicted from series-D trend; m05 unsynth → probe_unavailable, required to predict).
- Task 2 lead: m09, lead_qed_measured: 0.5826 (real measure_qed; highest among psa≥40, series≠D).
  Caveat: m09's 0.5826 is below the 0.6 floor; no measurable candidate satisfies all three constraints
  within the budget. m09 is the closest real, verifiable choice.
