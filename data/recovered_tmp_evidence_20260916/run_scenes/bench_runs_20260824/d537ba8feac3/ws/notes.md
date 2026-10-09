# Notes — predict m05 QED and pick a backup lead

## Pool classification (33 molecules, paged via list_pool, 9 budget used on listing, 8 left for measurements)

Target **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` = methoxyphenyl benzamide, N-alkyl chain n=11 (count C's after N: 11). Never synthesized.

Four congeneric series, each a benzamide "Ar-C(=O)N-(CH2)n-CH3" differing only in the aryl group and chain length n:

### Series A — methoxyphenyl (m05's series): `COc1ccc(cc1)C(=O)N-(C)n`
- m15 n=2  (mw179.22, alogp1.445, psa38.33, rotb3, arom1)
- m24 n=3  (mw193.25, alogp1.835, psa38.33, rotb4, arom1)
- m28 n=4  (mw207.27, alogp2.225, psa38.33, rotb5, arom1)
- m17 n=5  (mw221.30, alogp2.615, psa38.33, rotb6, arom1)
- m22 n=6  (mw235.33, alogp3.005, psa38.33, rotb7, arom1)
- m05 n=11 (target, unsynth; mw305.46, alogp4.956, psa38.33, rotb12, arom1)
  - Consistency checks pass: each CH2 adds ~14 mw, ~0.39 alogp, +1 rotb. n=11 from n=2 = +9*14=+126 → 305 ✓; +9*0.39=+3.51 → 4.96 ✓; rotb 3+9=12 ✓.

### Series B — nitrophenyl: `O=[N+]([O-])c1ccc(cc1)C(=O)N-(C)n` (all psa 72.24)
- m09 n=2, m02 n=3, m12 n=4, m32 n=5, m33 n=6, m29 n=7, m31 n=8, m06 n=9, m10 n=10

### Series C — sugar tail: `OCC(O)C(O)C(O)C(=O)N-(C)n` (all psa 110.02, arom 0)
- m14 n=2, m27 n=3, m01 n=4, m19 n=5, m03 n=6, m21 n=7, m18 n=8, m07 n=9, m25 n=10

### Series D — biphenyl: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-(C)n` (all psa 29.1)
- m13 n=2, m11 n=3, m04 n=4, m30 n=5, m23 n=6, m20 n=7, m26 n=8, m08 n=9, m16 n=10

## Job 1 — predict m05 (n=11) QED
Congeners of m05 (pool members in same series): **m15, m24, m28, m17, m22** (n=2..6). Target n=11 is far beyond measured range → extrapolation. m05 measured → probe_unavailable (confirmed unsynth).

Measured QED for methoxyphenyl series (n=2..6):
- n=2 m15: 0.7621
- n=3 m24: 0.7919  ← peak (alogp1.835 still in logP sweet spot)
- n=4 m28: 0.7518
- n=5 m17: 0.7498
- n=6 m22: 0.7377
Trend: peak at n=3, then steady decline (marginal steps -0.040, -0.002, -0.012; noisy but ~-0.018/n average n=3→6).

**Extrapolation to n=11 (Δn=+5 beyond last measured, +8 beyond peak):**
m05 descriptors at n=11: mw305.46, alogp4.956, psa38.33, rotb12, arom1. Between n=6 and n=11 the molecule crosses several QED penalty thresholds: alogp 3.0→5.0 (well into lipophilicity penalty), rotb 7→12 (>10 flexibility penalty), mw 235→305 (>300 weight penalty). So the decline should steepen somewhat vs the n=3→6 trend.

QED = geometric mean (8th root) of 8 property weights. Only MW, ALogP, RotB vary with n here; PSA(38.33), HBD(1), HBA(2), Arom(1), alerts are constant (methoxybenzamide has no Brenk alert). The 8th-root structure STRONGLY dampens individual penalties: a single weight's factor f only changes QED by f^(1/8).

Estimating the varying weights at n=11 vs n=6 (declining-weight factors): d_MW 0.93/1.0≈0.93, d_ALogP(5)≈0.6 vs (3)≈1.0→0.6, d_RotB(12)≈0.5 vs (7)≈0.85→0.59. Variable-product ratio n11/n6 ≈ 0.93·0.6·0.59 ≈ 0.33; 8th root ≈ 0.33^0.125 ≈ 0.87. → QED(n11) ≈ 0.7377·0.87 ≈ 0.64.
Linear fit n=3→6 (slope -0.01807/n) gives 0.7377 - 0.01807·5 = 0.647. Quadratic fit on n=2..6 (concave, accelerating decline) gives ~0.54 but is unstable over +5 extrapolation and overweights noise. Physically-grounded weight model + linear both land ~0.63-0.65; mild steepening pulls just below linear.

**Predicted QED(m05) ≈ 0.63** — clearly an extrapolation (well below nearest measured m22=0.738), accounts for threshold-crossing steepening but respects 8th-root dampening.

## Job 2 — lead: QED≥0.6 AND psa≥40 AND not methoxyphenyl
- psa filter kills methoxyphenyl (38.33<40, also excluded by series rule) and biphenyl (29.1<40).
- Survivors: nitrophenyl (psa72.24) and sugar (psa110.02) series.
- Measured m09 (nitrophenyl n=2, otherwise great: mw194 alogp1.345 rotb3 psa72) = **0.5826 < 0.6 → FAILS**. The nitro group is a Brenk alert in QED and penalizes the WHOLE nitrophenyl series (~0.18 below the methoxy analogue), so nitrophenyl peak (n=3, m02) is only borderline ~0.60 — risky.
- Sugar series (psa110 ideal, NO alerts, arom0): alogp is below the logP sweet spot at low n (too hydrophilic) but rises toward it: n=5 m19 alogp-1.632, n=7 m21 alogp-0.852, n=9 m07 alogp-0.072, n=10 m25 alogp0.318. No alerts → no alert penalty. Expected sugar QED ~0.68 (estimated via 8th-root weight model anchored to methoxy n=6=0.7377): better psa (110 vs 38) compensates partly for lower alogp/higher rotb/higher hbd(4). Clears 0.6 with margin.
- Budget after Job1+m09: 2 measurements left. Measure best sugar candidate; keep 1 as reserve/backup.

Lead candidate: **m21** (sugar n=7; mw263, alogp-0.852, psa110, rotb10, arom0) — balanced profile (mw safely <300, rotb moderate, alogp approaching sweet spot, no alerts, ideal psa). Backup if it fails: m25 (sugar n=10, best alogp) or m02 (nitro n=3, different mechanism).

### Job 2 measurement outcomes
- m09 (nitro n=2, alogp1.345) = **0.5826** → <0.6 FAILS.
- m21 (sugar n=7, alogp-0.852, psa110, no alerts) = **0.3297** → <<0.6 FAILS. The sugar series is far below expectations: the 4 HBD (polyol) + very negative alogp dominate despite ideal PSA and zero alerts. Even the best sugar member (m25 n=10 alogp+0.318) cannot recover from 0.33 to 0.6 (8th-root dampening caps a single-property gain at ~1.17× → ~0.39). **Sugar series ruled out.**
- This leaves nitrophenyl as the only viable series (biphenyl psa29.1<40 killed). The methoxyphenyl series peaked at n=3 (balance of alogp vs mw/rotb); nitrophenyl should peak at n=3 too. m02 (nitro n=3, alogp1.735 — closer to logP optimum than m09 n=2) is the best remaining shot.
- **m02 = 0.6046 ≥ 0.6 → PASSES.** Budget now exhausted (17/17).

### Final lead: m02
Constraints verified:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. not in m05's series (nitrophenyl, not methoxyphenyl) ✓

## Deliverable summary
- target_qed (m05, n=11) = **0.63** (extrapolation beyond measured n=2..6; mild steepening from threshold-crossing on alogp/mw/rotb, bounded by 8th-root dampening).
- congener_ids (m05's series, n=2..6): m15, m24, m28, m17, m22.
- lead = m02, lead_qed_measured = 0.6046.

## Measurements log (17/17 budget)
Listing: pages 1-9 (9). measure: m05(unavail,0), m15, m24, m28, m17, m22, m09, m21, m02 = 8 measured + 9 list = 17.
