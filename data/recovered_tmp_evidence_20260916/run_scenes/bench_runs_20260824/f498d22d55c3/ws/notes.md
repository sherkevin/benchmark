# Notes — compound triage

## Target
- measured logS in [-3, -2] AND measured bbb = 1.
- Budget: 8 assays total.

## Reasoning framework
- BBB favors: low TPSA (<~70), low HBD, low MW, moderate clogp.
- logS in [-3,-2] = moderately low solubility. Too polar (low clogp / high TPSA) → logS > -2.
  Too lipophilic (clogp >~4, esp with high MW) → logS < -3.

## Pre-screen ranking (best combo of plausible BBB + plausible logS)
Strong candidates (both plausible):
- z003001: MW238 clogp2.9 TPSA29 HBD1 — BBB yes; logS ~-2.5 likely. TEST.
- z002746: MW231 clogp1.55 TPSA30 HBD0 — BBB yes; logS ~-2. TEST.
- z002170: MW250 clogp3.0 TPSA35 HBD0 — BBB yes; logS ~-3 borderline. TEST.
- z001999: MW299 clogp1.5 TPSA42 HBD1 — BBB yes; logS ~-2. TEST.
- z002003: MW309 clogp3.58 TPSA43 HBD0 — BBB yes; logS ~-3.5 risk. TEST.

Borderline / backups:
- z002040: MW217 clogp1.77 TPSA46 HBD1 — BBB ok; logS maybe >-2 risk.
- z002052: MW369 clogp1.99 TPSA65 HBD0 — BBB ok-ish; logS borderline.
- z002764: MW318 clogp3.56 TPSA67 HBD2 — BBB marginal; logS <-3 risk.

Likely fails (one or both criteria off):
- z000889 (clogp8.84 → logS << -3), z002010/z002011/z002015/z002891 (clogp>4 → logS<-3),
- z001510/z001543/z002730/z015334 (very polar → logS>-2, many BBB-fail),
- z000364/z002216/z002683 (TPSA>80 / high HBD → BBB fail).

## Assay results (budget 8)
1. z003001: logS -0.137, bbb 1 → FAIL (logS > -2). (small flexible lactam very soluble despite clogp2.9)
2. z002746: logS -0.629, bbb 1 → FAIL (logS > -2)
3. z002170: logS -2.92, bbb 1 → ✓ QUALIFIES
4. z001999: logS -1.522, bbb 1 → FAIL (logS > -2). (morphine)
5. z002003: logS -3.6, bbb 1 → FAIL (logS < -3)

Recalibration: logS not purely clogp-driven. Small flexible molecules (z003001 clogp2.9 → -0.14)
stay soluble. logS in [-3,-2] found at clogp~3.0 MW250 (z002170=-2.92); clogp3.58 MW309 → -3.6.
So target clogp ~3.0-3.4 moderate MW, OR known low-solubility scaffolds (steroids, acetylated natural products).

## Round 2 picks (3 assays left)
6. z002052 — diacetylmorphine (z001999 morphine=-1.52; acetylating 2 OH → HBD1→0, clogp 1.5→1.99,
   MW +70). Should drop logS into ~[-2.5,-2]; heroin-class crosses BBB. Best bet.
7. z002764 — clogp3.56 like z002003(-3.6) but 2 phenolic OH (+24 TPSA, +2 HBD) → raise logS into range;
   BBB borderline (warfarin-like). Decent.
8. z002891 — steroid (clogp4.72, MW314, TPSA34, HBD0). Steroids cross BBB; rigid rings → poorly soluble,
   logS borderline-low. Long-shot.

## Round 2 results
6. z002052: logS -2.7894, bbb 1 → ✓ QUALIFIES (diacetylmorphine, as predicted)
7. z002764: logS -2.9008, bbb 0 → FAIL (bbb=0; TPSA/HBD too high for BBB as expected)
8. z002891: logS -4.5526, bbb 0 → FAIL (both off; steroid too insoluble, BBB fail)

## Final
Budget 8/8 used. Qualifying compounds (both logS∈[-3,-2] AND bbb=1, confirmed by real assays):
- z002170 (logS -2.92, bbb 1)
- z002052 (logS -2.7894, bbb 1)

Untested compounds are NOT confirmed and excluded from answer per task rules.
