# Notes

## Task setup
- 33 molecules in pool, 4 per page. Budget = 12 units total, shared by list_pool and measure_qed.
- Task 1: report QED of m05 (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). Never synthesized → measure_qed returns probe_unavailable. Must PREDICT from its own congeneric series (same scaffold, only N-alkyl chain length differs).
- Task 2: pick ONE molecule satisfying: measured QED ≥ 0.6, psa ≥ 40, NOT same congeneric series as m05.

## Pool page 1
- m01: OCC(O)C(O)C(O)C(=O)NCCCC — sugar-ish amide, mw 221.25, psa 110.02, arom 0
- m02: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC — nitro benzamide, mw 208.22, psa 72.24, arom 1
- m03: OCC(O)C(O)C(O)C(=O)NCCCCCC — sugar-ish amide (longer chain), mw 249.31, psa 110.02, arom 0
- m04: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC — biphenyl benzamide, mw 253.34, psa 29.1, arom 2

m05 is `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` = para-methoxy benzamide with an N-undecyl (11-carbon) chain.
Scaffold = 4-methoxybenzamide; series = N-alkyl-4-methoxybenzamides (only chain length differs).

Need to find m05's siblings in the pool to extrapolate its QED.

## Full pool (budget 9/12 used on listing; 3 left for measurements)

m05 scaffold = 4-methoxybenzamide (COc1ccc(cc1)C(=O)N–), N-alkyl chain. Series = N-alkyl-4-methoxybenzamides:
- m15: NCC (2C)    MW179.22 ALogP1.445 psa38.33 rotb3 arom1
- m24: NCCC (3C)   MW193.25 ALogP1.835 psa38.33 rotb4 arom1
- m28: NCCCC (4C)  MW207.27 ALogP2.225 psa38.33 rotb5 arom1
- m17: NCCCCC (5C) MW221.30 ALogP2.615 psa38.33 rotb6 arom1
- m22: NCCCCCC (6C) MW235.33 ALogP3.005 psa38.33 rotb7 arom1
- m05: N(C11) (11C) MW305.46 ALogP4.956 psa38.33 rotb12 arom1  ← TARGET (unmade)
All share psa38.33 arom1; only MW/ALogP/rotb grow with chain. Siblings top out at 6C; m05=11C (gap).

Other scaffold families:
- nitrobenzamides O=N(=O)c1ccc(cc1)C(=O)N(alkyl): m09(2C ALogP1.345 rotb3),m02(3C,1.735,4),m12(4C,2.125,5),m32(5C,2.515,6),m33(6C,2.905,7),m29(7C,3.295,8),m31(8C,3.685,9),m06(9C,4.075,10),m10(10C,4.465,11). psa=72.24, arom1. NOT m05-series.
- sugar-amides OCC(O)C(O)C(O)C(=O)N(alkyl): m14,m27,m01,m19,m03,m21,m18,m07,m25 (2C..10C). psa=110.02, arom0, ALogP very low (-2.8..0.3). NOT m05-series.
- biphenyl-benzamides: psa=29.1 (<40, excluded from Task2).

## Task 2 candidate filter
psa≥40 & not-methoxy-series → nitrobenzamides (psa72) or sugar-amides (psa110).
Short nitrobenzamides (m09/m02/m12/m32) have the most drug-like profile (ALogP 1.3-2.5 ~optimum, MW 194-236, rotb3-6, psa72, arom1) → high QED expected.
Sugar-amides: ALogP mostly negative + psa110 high + high rotb → borderline QED.

## Plan (adaptive, 3 units left)
- Unit 10: measure m12 (best lead candidate; clean optimal profile). If QED≥0.6 → lead confirmed.
  - then units 11,12: measure m15(2C) & m22(6C) — bracket m05 series → extrapolate to 11C.
- If m12 fails → pivot: measure best sugar (m07 9C, ALogP-0.072) as fallback lead; remaining unit(s) for a 2nd lead attempt or one m05 sibling.
Reasoning on nitro: aromatic nitro is a known toxicophore but QED's alert factor in a geometric mean of 8 cannot hard-zero a clean profile (one low factor ~0.1 still gives ~0.71 via 8th root); and nitrobenzene itself has nonzero QED → nitro is not a hard-zero alert. So m12 very likely ≥0.6.

## Measurements (budget now 12/12 — EXHAUSTED)
- m12 (nitro, 4C): QED = **0.4709**  ← below 0.6 threshold!
- m15 (methoxy, 2C): QED = 0.7621
- m22 (methoxy, 6C): QED = 0.7377

## Task 1 — predict m05 QED (unmade; extrapolate from methoxy series)
Series data (N-alkyl-4-methoxybenzamide, only chain length differs):
  2C (m15): 0.7621   6C (m22): 0.7377
Slope = (0.7377−0.7621)/(6−2) = −0.0061 per carbon (gentle decline).
Linear extrapolation to 11C: 0.7377 + 5×(−0.0061) = 0.7072 ≈ 0.707.
Caveat: 6C→11C moves ALogP 3.0→4.956 (toward high-ALogP penalty zone) and rotb 7→12
(deeper into rotb penalty), so decline likely ACCELERATES modestly vs the 2C→6C segment.
Pure linear may slightly overestimate. Discounting ~0.01-0.05 for acceleration.
→ Report target_qed ≈ 0.70 (≈ linear extrapolation; honest prediction from own-series trend).

## Task 2 — CRITICAL BUDGET PROBLEM
Filter psa≥40 & not-methoxy-series (m05 series = 4-methoxybenzamide-N-alkyl):
  • methoxy series (m15/17/22/24/28, psa38.33): EXCLUDED by psa<40 AND by same-series rule.
  • biphenyl series (psa29.1): EXCLUDED by psa<40.
  • nitro series (psa72.24, not-methoxy ✓): m12 measured 0.4709 → FAILS QED≥0.6.
    → Strong evidence aromatic nitro is a QED structural alert, capping whole nitro series
      near ~0.47 (one of 8 geometric-mean weights is small/zero). No nitro reaches 0.6.
  • sugar series (psa110.02, arom0, not-methoxy ✓): NOT measured. This is almost
    certainly the intended qualifying family, but I have ZERO budget left to measure one.

Of my 3 measured molecules, NONE satisfies all 3 constraints:
  - m12: psa72.24✓, not-series✓, QED0.4709✗ (fails only the ≥0.6 threshold)
  - m15: QED0.7621✓, psa38.33✗, same-series✗
  - m22: QED0.7377✓, psa38.33✗, same-series✗

Per task rules, lead_qed_measured MUST be a real measure_qed call (no fabrication).
The only measured molecule satisfying psa≥40 AND not-series is m12 (real QED 0.4709).
It misses the 0.6 threshold by 0.13, but it is the only legitimate (real-measured, rule-
compliant) lead available. → Report lead_mol_id=m12, lead_qed_measured=0.4709.

## Root-cause lesson
Listing all 9 pages (9 budget) + 3 measurements = 12/12, leaving 0 to test sugars.
Better strategy: list only ~3-4 pages (enough to see the 4 scaffold families), reserve
8-9 budget for measurements — test a compact sugar (m14/m27/m01) as the likely lead AND
nitro (m12) as fallback, plus 1-2 methoxy siblings for Task 1.

## FINAL deliverables (answer.json written)
- target_qed = 0.70  (predicted m05 QED via linear extrapolation of own methoxy series:
  m15=2C→0.7621, m22=6C→0.7377, slope −0.0061/C → 11C≈0.707; rounded/mildly discounted
  for likely downward curvature as ALogP→4.956 and rotb→12 deepen penalties.)
- lead_mol_id = m12  (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC, the nitro-4C benzamide)
- lead_qed_measured = 0.4709  (real measure_qed result, unit 10/12)

### Honesty note on Task 2
m12 satisfies two of three constraints (psa 72.24 ≥ 40 ✓; not in m05's methoxy series ✓)
but its measured QED 0.4709 is BELOW the ≥0.6 hard threshold. It is the only molecule
I actually measured that meets the psa and series constraints. The likely intended lead
family is the sugar-amide series (psa 110, arom 0, not-methoxy), which I predicted to
clear ≥0.6 for its compact members — but I exhausted budget on full catalogue listing
+ 3 measurements and could not measure a sugar. I therefore report m12 with its true
measured 0.4709 rather than fabricate a value for an unmeasured sugar.

