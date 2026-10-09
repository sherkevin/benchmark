# Notes

## Task Summary
- 20 compounds in candidates.tsv
- Need to find compounds with: logS between -3 and -2 (inclusive) AND bbb = 1
- Budget: 8 assays (measure_admet calls)
- Each assay returns logS and bbb for one compound

## Strategy
I need to prioritize compounds most likely to meet the criteria:
- logS between -3 and -2: moderate aqueous solubility (not too high, not too low)
- bbb = 1: blood-brain barrier penetration

Based on medicinal chemistry principles:
- BBB penetration favors: moderate clogp (2-5), low TPSA (<90), low HBD (<3)
- Moderate solubility (logS -3 to -2): moderate clogp, not too lipophilic

Let me analyze the candidates:

| mol_id | clogp | tpsa | hbd | Notes |
|--------|-------|------|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | Good BBB candidate |
| z000889 | 8.84 | 29.5 | 1 | Too lipophilic, poor solubility |
| z001510 | -1.04 | 72.7 | 1 | Low clogp, may have high solubility |
| z001543 | -1.64 | 119.6 | 5 | High TPSA, high HBD - poor BBB |
| z001999 | 1.5 | 41.9 | 1 | Good BBB candidate |
| z002003 | 3.58 | 43.1 | 0 | Good BBB candidate |
| z002010 | 4.28 | 29.5 | 0 | Good BBB candidate |
| z002011 | 4.32 | 30.2 | 0 | Good BBB candidate |
| z002015 | 4.14 | 23.6 | 0 | Good BBB candidate |
| z002040 | 1.77 | 46.2 | 1 | Good BBB candidate |
| z002052 | 1.99 | 65.1 | 0 | Good BBB candidate |
| z002170 | 3.0 | 34.9 | 0 | Good BBB candidate |
| z002216 | 0.86 | 98.0 | 2 | High TPSA - poor BBB |
| z002683 | 2.55 | 100.7 | 2 | High TPSA - poor BBB |
| z002730 | 0.09 | 83.6 | 1 | Low clogp, may have high solubility |
| z002746 | 1.55 | 30.2 | 0 | Good BBB candidate |
| z002764 | 3.56 | 66.8 | 2 | Good BBB candidate |
| z002891 | 4.72 | 34.1 | 0 | Good BBB candidate |
| z003001 | 2.9 | 29.1 | 1 | Good BBB candidate |
| z015334 | 0.74 | 81.6 | 1 | Moderate BBB candidate |

## Selection for Assays
I'll prioritize compounds with:
- clogp between 2-4 (moderate lipophilicity for both solubility and BBB)
- TPSA < 90 (for BBB penetration)
- HBD <= 1 (for BBB penetration)

Top candidates to assay (8 total):
1. z000364 - clogp 2.87, tpsa 83.1
2. z001999 - clogp 1.5, tpsa 41.9
3. z002003 - clogp 3.58, tpsa 43.1
4. z002010 - clogp 4.28, tpsa 29.5
5. z002011 - clogp 4.32, tpsa 30.2
6. z002015 - clogp 4.14, tpsa 23.6
7. z002170 - clogp 3.0, tpsa 34.9
8. z003001 - clogp 2.9, tpsa 29.1

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z000364 | -0.9482 | 0 | No (logS too high, bbb=0) |
| z001999 | -1.522 | 1 | No (logS > -2) |
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002010 | -5.0097 | 1 | No (logS < -3) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002015 | -3.82 | 1 | No (logS < -3) |
| z002170 | -2.92 | 1 | **YES** |
| z003001 | -0.137 | 1 | No (logS > -2) |

## Conclusion

Only **z002170** qualifies:
- logS = -2.92 (within -3 to -2 range)
- bbb = 1

Budget used: 8/8 assays
