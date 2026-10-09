# Notes - Compound Triage

## Task
- 20 compounds in data/candidates.tsv
- Budget: 8 assays (measure_admet calls)
- Qualifying criteria: logS between -3 and -2 (inclusive) AND bbb = 1

## Strategy
Need to select compounds likely to have:
- Moderate solubility (logS -3 to -2)
- BBB penetration (bbb=1)

BBB penetration typically favors:
- Lower TPSA (< 90 Å²)
- Moderate clogp (2-5)
- Fewer HBD

Solubility (logS -3 to -2) typically:
- Not too lipophilic (clogp not too high)
- Not too polar (would be too soluble)

## Compound Analysis

| mol_id | clogp | tpsa | hbd | Notes |
|--------|-------|------|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | Moderate clogp, tpsa borderline for BBB |
| z000889 | 8.84 | 29.5 | 1 | Very high clogp - likely poor solubility |
| z001510 | -1.04 | 72.7 | 1 | Low clogp - likely too soluble |
| z001543 | -1.64 | 119.6 | 5 | Very polar - poor BBB, high solubility |
| z001999 | 1.5 | 41.9 | 1 | Good BBB profile, moderate clogp |
| z002003 | 3.58 | 43.1 | 0 | Good BBB profile |
| z002010 | 4.28 | 29.5 | 0 | Good BBB profile, higher clogp |
| z002011 | 4.32 | 30.2 | 0 | Good BBB profile, higher clogp |
| z002015 | 4.14 | 23.6 | 0 | Good BBB profile |
| z002040 | 1.77 | 46.2 | 1 | Moderate profile |
| z002052 | 1.99 | 65.1 | 0 | Moderate profile |
| z002170 | 3.0 | 34.9 | 0 | Good BBB profile |
| z002216 | 0.86 | 98.0 | 2 | High tpsa - poor BBB |
| z002683 | 2.55 | 100.7 | 2 | High tpsa - poor BBB |
| z002730 | 0.09 | 83.6 | 1 | Low clogp - likely too soluble |
| z002746 | 1.55 | 30.2 | 0 | Good BBB profile |
| z002764 | 3.56 | 66.8 | 2 | Moderate profile |
| z002891 | 4.72 | 34.1 | 0 | Higher clogp - may have poor solubility |
| z003001 | 2.9 | 29.1 | 1 | Good BBB profile |
| z015334 | 0.74 | 81.6 | 1 | Moderate profile |

## Selection for Assays (8 compounds)
Focusing on compounds with:
- TPSA < 70 (better BBB chance)
- clogp between 1.5 and 4.5 (balanced solubility)

Selected:
1. z001999 - clogp 1.5, tpsa 41.9
2. z002003 - clogp 3.58, tpsa 43.1
3. z002010 - clogp 4.28, tpsa 29.5
4. z002015 - clogp 4.14, tpsa 23.6
5. z002040 - clogp 1.77, tpsa 46.2
6. z002170 - clogp 3.0, tpsa 34.9
7. z002746 - clogp 1.55, tpsa 30.2
8. z003001 - clogp 2.9, tpsa 29.1

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO - logS > -2 |
| z002003 | -3.6 | 1 | NO - logS < -3 |
| z002010 | -5.0097 | 1 | NO - logS < -3 |
| z002015 | -3.82 | 1 | NO - logS < -3 |
| z002040 | -2.3374 | 1 | **YES** |
| z002170 | -2.92 | 1 | **YES** |
| z002746 | -0.6286 | 1 | NO - logS > -2 |
| z003001 | -0.137 | 1 | NO - logS > -2 |

## Conclusion
Qualifying compounds (logS in [-3, -2] AND bbb=1):
- z002040
- z002170

Budget exhausted: 8/8 assays used.
