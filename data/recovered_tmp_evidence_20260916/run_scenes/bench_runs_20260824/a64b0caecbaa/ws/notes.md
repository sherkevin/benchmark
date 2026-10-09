# Triage notes

## Target profile
- logS in [-3, -2] (moderate aqueous solubility)
- bbb = 1 (brain penetrant)

## Heuristics
- BBB requires lowish polarity: TPSA < ~70, few HBD (0-1), MW not too high, clogp moderate (~1-4).
  High TPSA (>80) or many HBD strongly disfavors BBB.
- logS -3..-2 is moderate solubility. Very polar compounds (high TPSA, many HBD, clogp < 0) tend to
  be MORE soluble (logS > -2, miss upper bound). Very lipophilic (clogp > 5) tend to be insoluble (logS < -3, miss lower bound).
- Sweet spot for BOTH: clogp ~1-3.5, TPSA ~25-60, HBD 0-1, MW 180-360, rings/rotB moderate.

## Per-compound assessment (cheap descriptors)
| mol_id | mw | clogp | tpsa | hbd | hba | rotb | rings | BBB? | logS? | tier |
|--------|----|-------|------|-----|-----|------|-------|------|------|------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 5 | 3 | TPSA 83 borderline/high; BBB risky | solubility moderate (clogp2.87) ~ -3 maybe | medium |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 12 | 2 | TPSA low good but rotb 12, MW high, clogp 8.84 (too lipophilic) | logS very low (<-3, miss) | low |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 0 | 2 | xanthine-like caffeine; TPSA 72 borderline | clogp -1.04 -> logS > -2 (too soluble) | low |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 4 | 2 | TPSA 119, HBD 5 -> BBB no | too soluble (logS > -2) | low |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 1 | 5 | TPSA 42, clogp 1.5 -> BBB likely yes | logS ~ -2.5 good | HIGH |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 1 | 4 | TPSA 43, HBD 0, clogp 3.58 -> BBB likely yes | logS ~ -3.x borderline | HIGH |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 8 | 2 | TPSA 29, HBD0, clogp4.28 -> BBB yes | logS ~ -3.5 too low | medium |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 1 | 4 | TPSA 30, HBD0, clogp4.32 -> BBB yes | logS ~ -3.5 too low | medium |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 6 | 3 | TPSA 23, HBD0 -> BBB yes | clogp4.14 logS ~ -3.5 too low | medium |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | 2 | TPSA 46, clogp1.77 -> BBB yes | logS ~ -2.2 good | HIGH |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 2 | 5 | TPSA 65 borderline ok, HBD0, clogp2 -> BBB yes | logS ~ -2.5 good | HIGH |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 1 | 3 | TPSA 35, HBD0, clogp3 -> BBB yes | logS ~ -3 borderline-low | HIGH |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 3 | 2 | TPSA 98, HBD2 -> BBB no | too soluble | low |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 3 | 2 | TPSA 100, HBD2 -> BBB no | too soluble | low |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 1 | 3 | TPSA 83 borderline-high -> BBB risky | clogp0.09 logS > -2 too soluble | low |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | 2 | TPSA 30, HBD0, clogp1.55 -> BBB yes | logS ~ -2.3 good | HIGH |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 2 | 4 | TPSA 67, HBD2 -> BBB risky/borderline | logS ~ -3 borderline | medium |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 1 | 4 | TPSA 34, HBD0, clogp4.72 -> BBB yes | logS ~ -3.5 too low | medium |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | 2 | TPSA 29, clogp2.9 -> BBB yes | logS ~ -3 borderline | HIGH |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 5 | 3 | TPSA 81 borderline-high -> BBB risky | clogp0.74 logS > -2 too soluble | low |

## Tier selection
HIGH (best probability both conditions):
- z001999, z002003, z002040, z002052, z002170, z002746, z003001
That is 7 compounds. Budget = 8 assays. Add 1 medium-tier as 8th pick.

Medium candidates worth the 8th slot (BBB yes but logS borderline-low):
- z002010 (clogp4.28, logS likely too low) 
- z002011 (clogp4.32)
- z002015 (clogp4.14)
- z002891 (clogp4.72)
- z002764 (TPSA67/HBD2 BBB risky; pass on it)
- z000364 (TPSA83 BBB risky; pass)

Among medium, pick the one with best combined chance. All have clogp ~4.1-4.72 (logS likely < -3).
The HIGH tier also has z002170 (clogp3.0) and z003001 (clogp2.9) which are themselves borderline on logS.
z002003 (clogp3.58) also borderline on logS.

Decision: Assay the 7 HIGH-tier first, then use 8th assay on the most promising medium.
Actually re-rank: spend all 8 on the highest-value targets. The 7 HIGH + 1 best medium (z002010, lowest rotb2/HBD0/TPSA29 but clogp4.28). 

Let me reconsider the 8th. The borderline-LOW logS risk among HIGH: z002170(3.0), z003001(2.9), z002003(3.58).
These may fail logS< -3. So including a medium with clogp ~4.x (even worse) is not clearly better than just
covering all HIGH + 1 medium. Pick z002010 as 8th (BBB almost certain; only risk is logS too low).

Final 8 to assay: z001999, z002003, z002040, z002052, z002170, z002746, z003001, z002010.

## Assay results (budget 8/8 used)
| mol_id | logS | bbb | logS in [-3,-2]? | qualifies? |
|--------|------|-----|------------------|------------|
| z001999 | -1.522 | 1 | No (> -2, too soluble) | No |
| z002003 | -3.6 | 1 | No (< -3, too insoluble) | No |
| z002040 | -2.3374 | 1 | Yes | YES |
| z002052 | -2.7894 | 1 | Yes | YES |
| z002170 | -2.92 | 1 | Yes | YES |
| z002746 | -0.6286 | 1 | No (too soluble) | No |
| z003001 | -0.137 | 1 | No (too soluble) | No |
| z002010 | -5.0097 | 1 | No (too insoluble) | No |

Note: the logS heuristics from cheap descriptors were only loosely predictive. clogp ~1.5-2.0
compounds (z002746, z003001) turned out far more soluble than clogp would suggest, and
z002010 (clogp 4.28) was far less soluble. The 3 that qualified had clogp 1.77-3.0 and
TPSA 34-65, HBD 0-1 — the central "sweet spot."

## Conclusion
Qualifying IDs (confirmed by real assays): z002040, z002052, z002170.
