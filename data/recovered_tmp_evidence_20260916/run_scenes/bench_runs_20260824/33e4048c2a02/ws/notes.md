# Notes — QED extrapolation task

## Pool survey (9 pages, budget 9 used, 3 remaining)

Full catalogue read. Four scaffolds, each an N-alkyl chain-length series
(scaffold = aryl amide head group; chain length = N-(CH2)n-H varies):

| scaffold | head | members (by chain length n) |
|---|---|---|
| sugar amide | OCC(O)C(O)C(O)C(=O)N– | m14(2) m27(3) m01(4) m19(5) m21(7) m18(8) m07(9) m25(10) m03(6) |
| nitrobenzamide | O=[N+]([O-])c1ccc(cc1)C(=O)N– | m09(2) m12(4) m32(5) m33(6) m29(7) m31(8) m06(9) m10(10) m02(3) |
| biphenyl benzamide | c1ccc(cc1)-c1ccc(cc1)C(=O)N– | m13(2) m11(3) m04(4) m30(5) m23(6) m20(7) m26(8) m08(9) m16(10) |
| methoxybenzamide (m05's series) | COc1ccc(cc1)C(=O)N– | m15(2) m24(3) m28(4) m17(5) m22(6) m05(11,TARGET) |

m05 is the only methoxybenzamide never synthesized — it is the C11 extrapolation target.

## Task 1 — predict QED of m05

Target m05 = methoxybenzamide, N-alkyl chain C11 (mw 305.46, alogp 4.956, rotb 12).
Predict from the trend of its own series (same scaffold, only N-alkyl chain differs).
Available measured-able neighbors: m15(C2), m24(C3), m28(C4), m17(C5), m22(C6).

## Task 2 — pick a lead (QED≥0.6, psa≥40, NOT in m05's methoxy series)

Candidates NOT in methoxybenzamide series with psa≥40:
- sugar-amide series: m01,m03,m07,m14,m18,m19,m21,m25,m27 — psa 110, alogp negative.
  Drug-like? alogp very low, but QED unknown until measured.
- nitrobenzamide series: m02,m06,m09,m10,m12,m29,m31,m32,m33 — psa 72.24.
  m09(C2, mw194, alogp1.345, rotb3) and m12(C4) and m02(C3) look most drug-like.

Plan: spend remaining 3 QED measurements.
- For Task 1 trend: measure m15(C2), m28(C4), m22(C6) spanning series range → extrapolate to C11.
- For Task 2: one of these same measurements may double as a lead candidate? No —
  methoxy series is excluded from Task 2. So Task 2 needs its own measurement(s).
  Re-budget: only 3 measurements left. Must serve both tasks.

Decision: measure m15 (C2 methoxy, Task1 trend + low end), m22 (C6 methoxy, Task1 trend + high end),
and use the third measurement for a Task-2 candidate. For Task1, two points + the known
shape of QED vs chain length (QED falls as rotb/logp grow) should let a reasonable extrapolation.

Better: measure m28(C4) as third methoxy point → 3 points define the methoxy trend for Task1,
but then NO measurement left for Task2 lead → cannot report measured QED for lead. Not allowed.

Resolution: Task2 requires a MEASURED lead QED. So I MUST reserve ≥1 measurement for a Task2
candidate. Therefore spend at most 2 on the methoxy trend. Use m15(C2) and m22(C6) as the two
ends of the methoxy series → linear extrapolation to C11. Third measurement: a Task2 candidate.

Task2 candidate choice: want QED≥0.6 & psa≥40. Nitrobenzamides (psa 72.24): pick the most
drug-like by descriptors. m09 (C2): mw194, alogp1.345, rotb3 — best MW/rotb. Also m02(C3)
mw208 alogp1.735 rotb4. m09 looks most favorable. But sugar amides have psa 110 and very low
alogp; some have decent MW e.g. m27(C3, mw207) — but alogp −2.4 likely low QED.

Pick m09 as Task2 lead candidate to measure (3rd measurement).

## Measurements performed (budget used = 12/12)

| mol_id | scaffold | n | mw | logP | rotb | psa | QED |
|---|---|---|---|---|---|---|---|
| m15 | methoxy | 2 | 179.22 | 1.445 | 3 | 38.33 | 0.7621 |
| m22 | methoxy | 6 | 235.33 | 3.005 | 7 | 38.33 | 0.7377 |
| m09 | nitro | 2 | 194.19 | 1.345 | 3 | 72.24 | 0.5826 |

## Task 1 — predict QED of m05 (methoxy, n=11)

Series points: m15 (n2, QED0.7621), m22 (n6, QED0.7377). Descriptors linear in n:
MW=151.2+14.0n, logP=-3.58+0.39n, rotb=3+n. (m05: mw305.5, logP4.956, rotb12 ✓.)

Linear extrapolation slope = (0.7377-0.7621)/4 = -0.0061/carbon → at n=11: 0.7377-0.0305 = 0.707.
BUT trend must be convex (declining): from n2→n6 logP moved 1.45→3.0 (crossing peak ~2.5, net
~neutral for d_ALOGP) so the -0.0061/carbon was driven mostly by rotb+MW trade. From n6→n11
logP goes 3.0→4.956 (ALL past peak → d_ALOGP declines steeply, crosses the logP~5 penalty
threshold) and rotb 7→12 (high-penalty zone), while MW 235→305 stays near-optimal (slight
benefit). So decline accelerates → QED(m05) clearly below the 0.707 linear estimate.
Component reasoning (logP d drops ~0.95→~0.5, rotb d ~0.82→~0.55, MW ~flat) → ~0.65-0.69.
**Estimate: target_qed ≈ 0.68** (below linear 0.707, reflecting accelerating logP/rotb penalties
at the long chain; MW stays favourable so not lower).

## Task 2 — lead (QED≥0.6, psa≥40, NOT methoxy series)

Methoxy series all psa 38.33 < 40 → excluded. Biphenyl series all psa 29.1 < 40 → excluded.
Nitrobenzamide series psa 72.24 ✓, but m09 (shortest chain, BEST descriptors: mw194/logP1.345/
rotb3) only measured 0.5826 < 0.6 → nitro group likely carries a structural alert capping the
whole series below 0.6. So nitro is out.
=> Task 2 answer MUST be a SUGAR AMIDE (OCC(O)C(O)C(O)C(=O)N–), psa 110.02 ✓, no nitro alert.

Sugar amides: polar head fixed (HBD4/HBA5/PSA110/arom0); chain trades logP (very negative,
improves with n) vs rotb (grows with n) vs MW (peaks ~n6-10). logP is the most-penalized
component (all below d_ALOGP peak), so improving it matters most → longer chains favoured
until rotb explodes (~n10+). Crude d_ALOGP×d_ROTB×d_MW product by n:
 m14(n2)0.250, m27(n3)0.263, m01(n4)0.274, m19(n5)0.285, m03(n6)0.287, m21(n7)0.291,
 m18(n8)0.272, m07(n9)0.256. Peak ≈ m21 (n7).
Best candidate = m21 (mw263, logP-0.852, rotb10, psa110).

## Budget problem — OUTCOME
Attempted measure_qed(m21) (best sugar-amide candidate): returned budget_exhausted (12/12 used).
Cannot obtain any sugar-amide measurement.

Diagnosis of strategy error: I paged ALL 9 pages (9 units) "to see the whole pool", leaving
only 3 units for measurements. The task hint explicitly said page minimally ("you do not
have to see every candidate before you start measuring"). Had I stopped paging after ~5-6
pages (enough to identify the methoxy series + a couple of viable scaffolds), I would have
had 6-7 measurement units — enough to fit the methoxy trend (3 pts) AND probe sugar amides
(2-3 pts) to find a ≥0.6 lead.

## Which scaffold can actually satisfy all 3 Task-2 constraints?
By elimination on psa (catalogue values):
- methoxy series (m05,m15,m17,m22,m24,m28): psa 38.33 < 40 → EXCLUDED (constraint 2).
- biphenyl series (m04,m08,m11,m13,m16,m20,m23,m26,m30): psa 29.1 < 40 → EXCLUDED.
- nitro series (m02,m06,m09,m10,m12,m29,m31,m32,m33): psa 72.24 ✓. But nitro is a strong
  structural alert. Back-calibrate d_NOPK from m09: QED=0.5826 with d_MW(194)≈0.27,
  d_ALOGP(1.445)≈0.6, d_ROTB(3)≈0.8 → d_NOPK≈0.055 (near-zero, severe). Under that, the
  nitro series MAX (mid-chain at logP peak ~2.5, e.g. m32) ≈ 0.597 — still < 0.6. So nitro
  is capped below 0.6 across the WHOLE series.
- sugar series (m01,m03,m07,m14,m18,m19,m21,m25,m27): psa 110.02 ✓, no nitro alert, HBD4/
  arom0. Only scaffold that CAN reach ≥0.6. Best candidate by descriptor reasoning = m21
  (n7, mw263, logP-0.852, rotb10): logP closest to desirable among sugars while rotb not yet
  extreme; crude d_MW×d_ALOGP×d_ROTB product peaks here. **True Task-2 lead is a sugar amide
  (m21 best guess) — but I have no measurement to confirm.**

## What I can honestly report
Real measure_qed results: m15=0.7621, m22=0.7377, m09=0.5826.
Rule: "Every QED reported as measured must come from a real measure_qed call."
- m15/m22: real QED ≥0.6 BUT psa 38.33 <40 AND in m05's methoxy series → fail 2 constraints.
- m09: real QED 0.5826 <0.6 (fails constraint 1 by 0.017), BUT psa 72.24≥40 ✓ and not
  methoxy ✓ (satisfies 2 of 3). Closest to qualifying of the three I measured.
Cannot fabricate m21's QED (would violate the real-measurement rule). So lead = m09 with its
real measured 0.5826, acknowledging it falls short of the 0.6 bar — a direct consequence of
over-paging leaving no budget to measure a sugar amide.

## Final values for answer.json
- target_qed (m05): Linear extrapolation of the methoxy trend (m15 n2→0.7621, m22 n6→0.7377):
  slope -0.0061/carbon → at n11 = 0.707. Tempered for accelerating logP/rotb decline past n6
  (logP 3.0→4.956 crosses the ~5 penalty; rotb 7→12 enters heavy-penalty zone), partially
  offset by MW reaching the optimal ~300 window. Net ≈ 0.70. → report 0.70.
- lead_mol_id: "m09" (only measured molecule satisfying psa≥40 and not-methoxy; fails 0.6 bar).
- lead_qed_measured: 0.5826 (real measure_qed value for m09).

Caveat: the intended Task-2 lead is almost certainly a sugar amide (m21), which would need a
real measurement I could not afford. Reporting m09 is the honest fallback with a real number.
