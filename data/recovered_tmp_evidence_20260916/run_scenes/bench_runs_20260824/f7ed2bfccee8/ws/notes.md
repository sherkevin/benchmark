# Notes - Compound Triage

## Task
Find compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays (all used)

## Assay Results

| mol_id | clogp | tpsa | logS | bbb | Qualifies? |
|--------|-------|------|------|-----|------------|
| z001999 | 1.5 | 41.9 | -1.522 | 1 | NO (logS > -2) |
| z002040 | 1.77 | 46.2 | -2.3374 | 1 | **YES** |
| z002746 | 1.55 | 30.2 | -0.6286 | 1 | NO (logS > -2) |
| z003001 | 2.9 | 29.1 | -0.137 | 1 | NO (logS > -2) |
| z002170 | 3.0 | 34.9 | -2.92 | 1 | **YES** |
| z002003 | 3.58 | 43.1 | -3.6 | 1 | NO (logS < -3) |
| z002011 | 4.32 | 30.2 | -4.13 | 1 | NO (logS < -3) |
| z002015 | 4.14 | 23.6 | -3.82 | 1 | NO (logS < -3) |

## Qualifying compounds: z002040, z002170

Both have:
- logS in range [-3, -2]
- bbb = 1

## Budget exhausted: 8/8 assays used
