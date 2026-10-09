# Task notes

## Pool structure (from group_by_scaffold, free)
4 congeneric series (scaffold + N-alkyl chain of varying length):

- **G1** `COc1ccc(cc1)C(=O)N` (methoxybenzamide) — CONTAINS m05 (target)
  - m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, unmeasurable)
- **G2** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide)
  - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **G3** `OCC(O)C(O)C(O)C(=O)N` (sugar amide)
  - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **G4** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenylamide)
  - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Strategy
- Budget = 17, shared by list_pool (1/page, 4 entries) and measure_qed (1 each).
- **Task 1**: measure G1 measurable members (chain 2,3,4,5,6), fit polynomial, evaluate at chain=11.
- **Task 2**: need measured QED ≥ 0.6, psa ≥ 40, NOT in G1.
  - G4 (biphenylamide) PSA ≈ amide only (~29) → <40, excluded.
  - G3 (sugar) PSA huge, but high MW/HBD → QED likely low; backup only.
  - G2 (nitrobenzamide) PSA = nitro(~34) + amide(~29) ≈ 63 ≥ 40 ✓; NOT G1 ✓. Best candidate.
  - Note: G1 (methoxybenzamide) PSA ≈ OMe ether(~9) + amide(~29) ≈ 38 < 40 → would fail
    Task 2's psa constraint anyway (doubly excluded, also same-series-as-m05 rule).

## G1 measurements (Task 1 data) — 5 used, budget 12 left
| mol | chain | QED    | MW    | ALOGP | ROTB  | ALERTS | (HBA .9305, HBD .9866, PSA .933, AROM .8273 constant) |
|-----|-------|--------|-------|-------|-------|--------|---|
| m15 | 2     | 0.7621 | 0.332 | 0.874 | 0.993 | 0.8425 |
| m24 | 3     | 0.7919 | 0.406 | 0.936 | 0.970 | 0.8425 |
| m28 | 4     | 0.7518 | 0.491 | 0.977 | 0.848 | 0.6397 |
| m17 | 5     | 0.7498 | 0.585 | 0.997 | 0.689 | 0.6397 |
| m22 | 6     | 0.7377 | 0.684 | 0.997 | 0.533 | 0.6397 |
Peak at chain 3, then decline (rotb + logp worsen faster than MW improves).

## G1 extrapolation to chain 11 (fits, unstable — long extrapolation)
- overall QED quad deg2: 0.5416 (captures peak; primary answer)
- overall QED linear deg1: 0.695 (monotonic, ignores ROTB collapse → too high)
- overall QED cubic deg3: 2.13 (overshoots wildly, reject)
- MW component quad@11: 1.31 (capped 1.0; real ~0.9, MW≈305 near optimal)
- ALOGP quad@11: 0.684; linear: 0.695 (logP rising past optimal ~2-3)
- ROTB quad@11: -1.05→0; linear: -0.034→0 (≈11 rot bonds, score collapses)
- ALERTS quad@11: 0.976 (spurious; real stays 0.64, structural alerts don't change)
Component-weighted geo-mean reconstruction gives ~0.43–0.56 depending on ROTB assumption;
clusters around ~0.5, consistent with the overall quad fit (0.54). Linear 0.70 too high.

## Anchor plan: measure G2 (nitrobenzamide, closest scaffold to G1)
G2 shares the benzamide+alkyl chain, so its QED-vs-chain DECAY SHAPE at long chain
informs G1's chain-11 behavior (offset only by head-group differences). G2 also serves
Task 2 (psa≈63≥40, not G1). Measure m02(ch3), m12(ch4) [Task 2 candidates + low-chain
anchor] and m10(ch10) [long-chain decay anchor for Task 1].

## G2 measurements — 3 used, budget 9 left
| mol | chain | QED    | MW    | ALOGP | ROTB  | ALERTS | (HBA .985, HBD .987, PSA .930, AROM .827 const) |
|-----|-------|--------|-------|-------|-------|--------|---|
| m02 | 3     | 0.6046 | 0.498 | 0.922 | 0.970 | 0.2414 |
| m12 | 4     | 0.4709 | 0.592 | 0.968 | 0.848 | 0.0817 |
| m10 | 10    | 0.3724 | 1.000 | 0.704 | 0.151 | 0.0817 |

### Cross-scaffold calibration (Task 1 anchor)
- ROTB component is IDENTICAL at matched chain across G1 & G2
  (G1 ch4=0.848, G2 ch4=0.848 ✓; both decay on same rot-bond curve).
  → G2 ch10 ROTB=0.151 ⇒ G1 ch11 ROTB ≈ 0.13 (validates the ROTB→~0 extrapolation).
- Head-group offset = ALERTS: nitro gives ALERTS≈0.24→0.08 (worst driver);
  methoxy gives ALERTS≈0.84→0.64. So G1 sits ~0.6× above G2 at long chain via ALERTS,
  but BOTH lose to ROTB collapse at long chain. Long-chain QED driven down by ROTB→~0
  in both series.
- ⇒ m05 (G1 ch11) QED pulled down chiefly by ROTB≈0.13 (like m10) and ALOGP≈0.68,
  partly rescued by high ALERTS (0.64) and MW≈1.0. Net ≈ 0.5, consistent with the
  overall quadratic fit 0.5416. Use 0.5416 as the Task-1 answer.

## Task 2 candidate status (need QED≥0.6 & psa≥40 & not G1)
- **m02 nitro ch3 QED=0.6046** ✓(≥0.6), psa≈63 ✓(≥40), not-G1 ✓ → CHOSEN LEAD.
- m09 ch2 = 0.5826 ✗ (MW 0.412 too low; ALERTS 0.241 same as m02).
- m12 ch4 = 0.4709 ✗, m33 ch6 = 0.4591 ✗ (ALERTS collapses to 0.082 at ch≥4).
- m10 ch10 = 0.3724 ✗. m21 G3 ch7 = 0.3297 ✗ (sugar: HBD .056, PSA .583 → hopeless).
- G2 ALERTS pattern: 0.241 at ch2-3, drops to 0.082 at ch≥4. So only ch2/3 nitro are
  viable; m02 (ch3) is the G2 peak (best MW/ROTB/ALERTS balance). m09 (ch2) too low-MW.
- G3 (sugar) hopeless (4 HBD). G4 (biphenyl) psa≈29 <40 excluded. G1 same-series excluded.
- ⇒ m02 is the best (and only confirmed) molecule meeting all three constraints.

## Final answers
- **Task 1 (m05 QED):** 0.5416 (quadratic fit on G1 ch2-6 overall QED, evaluated at ch11).
  Supported by: G2-ch10 real measurement (0.372, ROTB 0.151) anchoring long-chain ROTB
  collapse; component reconstruction (ROTB≈0.13, MW≈1.0, ALOGP≈0.68, ALERTS 0.64 →
  geo-mean ~0.5); ALERTS head-group offset (G1 0.64 >> G2 0.082) explaining why m05 > m10.
- **Task 2 (lead):** m02, measured QED = 0.6046 (nitrobenzamide ch3; psa≈63≥40; not G1).

## Catalogue verification (list_pool page 1, 1 budget)
- m02: mw 208.22, alogp 1.735, **psa 72.24** ✓(≥40), rotb 4, arom 1. Confirms lead valid.
- m04 (G4 biphenyl): psa 29.1 < 40 → G4 excluded (as predicted).
- m01/m03 (G3 sugar): psa 110.02 (very high) but QED crushed by HBD.

## Final constraint check for m02 (lead)
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. not same series as m05: m02 is G2 (nitrobenzamide), m05 is G1 (methoxybenzamide),
   different scaffold ✓

## Budget tally (final): 12 used, 5 left
measure_qed: m15,m24,m28,m17,m22,m02,m12,m10,m09,m33,m21 = 11
list_pool: page 1 = 1
Total = 12. (group_by_scaffold, qed_components, fit_trend all free.)
