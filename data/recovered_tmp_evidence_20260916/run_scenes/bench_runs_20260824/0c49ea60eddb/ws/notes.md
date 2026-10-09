# Notes - Compound Screening Task

## Task Summary
- 20 compounds in data/candidates.tsv
- Need to find compounds with: logS between -3 and -2 (inclusive) AND bbb = 1
- Budget: 8 assays total

## Strategy
For BBB penetration: TPSA < 90, clogp ~2-4 is favorable.
For solubility logS -3 to -2: moderate clogp (2-4) is ideal.

## Assay Results (5/8 used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z000364 | -0.9482 | 0 | No (logS too high, bbb=0) |
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002170 | -2.92 | 1 | No (logS < -3) |
| z002764 | -2.9008 | 0 | No (bbb=0) |
| z003001 | -0.137 | 1 | No (logS too high) |

## Remaining assays: 3

Need to find compounds with logS in [-3, -2] AND bbb=1. None found yet.
