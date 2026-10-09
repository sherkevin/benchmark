# Task notes

## Budget
12 measurements total, shared by Job 1 (predict m05 QED) and Job 2 (pick a lead).
Used 10, conserved 2 (no remaining useful target — see below).

## Pool structure — four congeneric series
Every molecule is a benzamide: `X-c1ccc(cc1)C(=O)N(CCCC...)`. The ring substituent X
is fixed within a series; only the N-alkyl chain length (n = #carbons after N) varies.
Each scaffold has its own QED-vs-n curve (cannot transfer between series).

| series | X | psa | members (n) |
|---|---|---|---|
| methoxy | OCH3 | 38.33 | m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target, unsynth)** |
| nitro | NO2 | 72.24 | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) |
| biphenyl | phenyl | 29.1 | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) |
| polyol | OCC(O)C(O)C(O)- | 110.02 | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) |

## Job 1 — m05 (methoxy series, n=11) — extrapolation
m05 is methoxy; pool holds only methoxy n=2..6, so this is extrapolation beyond range.

Measured methoxy QED vs n:
- n=2 (m15): 0.7621
- n=3 (m24): 0.7919  ← peak
- n=4 (m28): 0.7518
- n=5 (m17): 0.7498
- n=6 (m22): 0.7377

Trend (stated explicitly): an **inverted-U**: QED rises n=2→3 to a peak (0.7919 at n=3),
then a gentle **declining tail** n=3→6 (−0.054 total over 3 steps, avg ≈ −0.018/step,
noisy because QED is a product of stepwise desirability functions so thresholds cause
plateaus). The peak at n=3 reflects rotB/MW/ALogP all still in the desirable zone; the
tail declines as the chain pushes MW↑, ALogP↑, rotB↑ (PSA, arom, HBD/HBA, alerts stay
constant & good within the series — 5 of 8 QED properties are fixed).

Extrapolation to n=11 (5 steps past n=6): I do NOT assume a straight line. Anchored to
the measured n=6 value (0.7377), I scale by the weighted-desirability change of the 3
varying properties from n=6→n=11, using Bickerton QED property weights:
- ALogP 3.005→4.956 (desirability ~0.6→~0.30, weight 0.115): logΔ ≈ −0.69
- rotB 7→12 (~0.6→~0.45, weight 0.065): logΔ ≈ −0.29
- MW 235→305 (~0.95→~0.90, weight 0.165): logΔ ≈ −0.054
Weighted log-change ≈ 0.115·(−0.69)+0.065·(−0.29)+0.165·(−0.054) ≈ −0.117
=> QED(n=11) ≈ 0.7377 · exp(−0.117) ≈ 0.7377 · 0.890 ≈ **0.66**
(sensitivity: ALogP-desirability floor at 0.25–0.35 gives a ~0.65–0.67 band).

**Predicted target_qed (m05) = 0.66**

Note: the methoxy series has no pool members beyond n=6, so the 2 conserved measurements
cannot improve this extrapolation — no further methoxy points are obtainable.

## Job 2 — lead compound
Constraints: (1) measured QED ≥ 0.6, (2) psa ≥ 40, (3) not methoxy series.
Pre-filter on the free constraints (psa≥40 & not-methoxy) BEFORE measuring:
- methoxy (psa 38.33 < 40) and biphenyl (psa 29.1 < 40) are excluded entirely.
- candidates: nitro (psa 72.24) and polyol (psa 110.02) families.

Measured candidate QEDs:
- nitro n=2 (m09): 0.5826   (below 0.6)
- nitro n=3 (m02): **0.6046** ← satisfies all three
- nitro n=4 (m12): 0.4709   (series past peak)
- polyol n=2 (m14): 0.3237  (PSA 110 hurts drug-likeness)
- polyol n=3 (m27): 0.3330

**Chosen lead = m02** (nitro, n=3): QED 0.6046 ≥ 0.6 ✓, psa 72.24 ≥ 40 ✓,
nitro scaffold ≠ methoxy ✓. It sits at the nitro-series peak (n=3), and polyol is a
dead-end (0.32–0.33), so m02 is the best available lead.

## Deliverable values
- target_qed = 0.66
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids (methoxy = m05's series) = ["m15","m17","m22","m24","m28"]
