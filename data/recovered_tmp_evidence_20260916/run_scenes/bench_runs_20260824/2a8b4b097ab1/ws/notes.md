# Notes - Compound Triage

## Task
Find compounds that qualify with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays

## Selection Strategy
Prioritized compounds with:
- TPSA < 90 (better BBB penetration)
- clogP between 2-5 (moderate lipophilicity)

## Assays Run

| # | mol_id | clogP | TPSA | logS | bbb | Qualifies? |
|---|--------|-------|------|------|-----|------------|
| 1 | z000364 | 2.87 | 83.1 | -0.9482 | 0 | No (logS too high, bbb=0) |
| 2 | z002003 | 3.58 | 43.1 | -3.6 | 1 | No (logS < -3) |
| 3 | z002010 | 4.28 | 29.5 | -5.0097 | 1 | No (logS < -3) |
| 4 | z002011 | 4.32 | 30.2 | -4.13 | 1 | No (logS < -3) |
| 5 | z002015 | 4.14 | 23.6 | -3.82 | 1 | No (logS < -3) |
| 6 | z002170 | 3.0 | 34.9 | -2.92 | 1 | **YES** |
| 7 | z002764 | 3.56 | 66.8 | -2.9008 | 0 | No (bbb=0) |
| 8 | z003001 | 2.9 | 29.1 | -0.137 | 1 | No (logS > -2) |

## Results
- Total assays used: 8 (budget exhausted)
- Qualifying compounds: 1 (z002170)

## Conclusion
Only **z002170** meets both criteria:
- logS = -2.92 (within -3 to -2 range)
- bbb = 1
