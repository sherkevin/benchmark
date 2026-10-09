# Triage notes

## Goal
- 20 compounds in data/candidates.tsv
- Budget: 8 assays (measure_admet), each returns logS (qualifying: -3 to -2 inclusive) and bbb (qualifying: 1).
- Need to find compounds satisfying BOTH: logS in [-3, -2] AND bbb = 1.

## Heuristics
- BBB penetration favored by: low TPSA (<~70-90), low HBD (<~3), clogP ~2-4, mw < ~450, few rotb.
- High TPSA, high HBD, many HBA hurt BBB. Many polar groups also raise solubility (logS higher).
- logS in [-3,-2] is moderately low solubility -> usually needs some lipophilicity (clogP > ~2) but not extreme.

## Candidate analysis (by descriptors)
| mol_id | mw | clogp | tpsa | hbd | hba | rotb | rings | BBB-plausible? | solubility-plausible(-3..-2)? |
|---|---|---|---|---|---|---|---|---|---|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 5 | 3 | marginal (tpsa high, 6 hba) | yes (clogp 2.87) |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 12 | 2 | lipophilic, low tpsa -> BBB yes, but logS likely very low (<-3, clogp 8.84) | NO (too lipophilic) |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 0 | 2 | small, moderate tpsa -> maybe BBB | NO (too hydrophilic, logS>-2) |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 4 | 2 | NO (tpsa 119, hbd 5) | NO (too hydrophilic) |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 1 | 5 | yes (low tpsa, mw ok) | maybe (clogp 1.5 -> logS maybe -2.x) borderline |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 1 | 4 | yes (hbd 0, tpsa 43) | yes (clogp 3.58) |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 8 | 2 | yes (low tpsa, hbd 0) | yes (clogp 4.28) |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 1 | 4 | yes (hbd 0, tpsa 30) | yes (clogp 4.32) |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 6 | 3 | yes (very low tpsa, hbd 0) | yes (clogp 4.14) |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | 2 | yes | borderline (clogp 1.77) |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 2 | 5 | marginal (6 hba) | borderline |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 1 | 3 | yes | yes (clogp 3.0) |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 3 | 2 | NO (tpsa 98, hbd 2, 5 hba) | NO (too hydrophilic) |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 3 | 2 | NO (tpsa 100) | borderline |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 1 | 3 | NO (tpsa 83) | NO (clogp 0.09 hydrophilic) |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | 2 | yes | borderline |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 2 | 4 | marginal (hbd2, tpsa67) | yes |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 1 | 4 | yes (low tpsa, hbd 0) | yes (clogp 4.72) |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | 2 | yes | yes (clogp 2.9) |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 5 | 3 | NO (tpsa 81, 5 hba) | NO (hydrophilic) |

## Selection strategy
Pick 8 with best combined likelihood of BOTH criteria. Favor:
- BBB: low TPSA (<~70), HBD 0-1, clogP 2-4, mw<400.
- logS -3..-2: clogP roughly 2.5-4.5 (sweet spot), moderate polarity.

Top picks (rank-ordered):
1. z003001 — clogp 2.9, tpsa 29, hbd1, mw 238. Sweet spot for both.
2. z002170 — clogp 3.0, tpsa 35, hbd0, mw 250.
3. z002015 — clogp 4.14, tpsa 24, hbd0, mw 336. (logS maybe ~-3.x, borderline low end)
4. z002010 — clogp 4.28, tpsa 29, hbd0, mw 339.
5. z002011 — clogp 4.32, tpsa 30, hbd0, mw 326.
6. z002003 — clogp 3.58, tpsa 43, hbd0, mw 309.
7. z002891 — clogp 4.72, tpsa 34, hbd0, mw 314.
8. z000364 — clogp 2.87, tpsa 83 (high), but only BBB borderline; alternative z001999 (clogp1.5, tpsa42) borderline solubility.
   -> Choose z001999 over z000364 because tpsa 42 (better BBB) and rings/low rotb. But clogp 1.5 -> logS likely > -2.
   -> Better: z002764 (clogp 3.56, tpsa 67, hbd2) — borderline BBB but solubility good.
   -> Best 8th: z002764 (clogp 3.56 good for logS; tpsa 67 borderline BBB).

Final 8 to assay: z003001, z002170, z002015, z002010, z002011, z002003, z002891, z002764.

## Assay results
| mol_id | logS | bbb | qualifies? | reason |
|---|---|---|---|---|
| z003001 | -0.137 | 1 | NO | logS > -2 (too soluble) |
| z002170 | -2.92 | 1 | **YES** | -3 ≤ -2.92 ≤ -2 AND bbb=1 |
| z002003 | -3.6 | 1 | NO | logS < -3 (too insoluble) |
| z002010 | -5.0097 | 1 | NO | logS < -3 |
| z002015 | -3.82 | 1 | NO | logS < -3 |
| z002011 | -4.13 | 1 | NO | logS < -3 |
| z002891 | -4.5526 | 0 | NO | bbb=0 |
| z002764 | -2.9008 | 0 | NO | bbb=0 (logS -2.90 would qualify on solubility) |

Budget exhausted (8/8).

## Conclusion
Only **z002170** satisfies both criteria: logS = -2.92 (in [-3, -2]) and bbb = 1.

Note: z003001 had clogp 2.9 (predicted sweet spot) but measured logS = -0.137 (much more soluble than predicted). z002764 measured logS = -2.9008 (would have qualified on solubility) but bbb = 0. This shows descriptor-based prediction is imperfect; the dual constraint is genuinely restrictive. Among 8 measured, only z002170 met both.
