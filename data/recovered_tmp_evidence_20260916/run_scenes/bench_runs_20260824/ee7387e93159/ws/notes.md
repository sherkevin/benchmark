# Notes

## Task
Find compounds with logS between -3 and -2 (inclusive) AND bbb=1 from 20 candidates.
Budget: 8 assays.

## Analysis of candidates
Key criteria for BBB penetration: TPSA < ~60, clogP 2-5, low HBD
Key criteria for logS -3 to -2: moderate lipophilicity (clogP ~2-4), not too large

### Top candidates to assay (based on descriptor analysis):
1. z002003 - clogP=3.58, TPSA=43.1, HBD=0 → strong BBB candidate, decent solubility range
2. z002170 - clogP=3.0, TPSA=34.9, HBD=0 → strong BBB candidate
3. z003001 - clogP=2.9, TPSA=29.1, HBD=1 → strong BBB candidate
4. z002010 - clogP=4.28, TPSA=29.5, HBD=0 → strong BBB, might be too insoluble
5. z002011 - clogP=4.32, TPSA=30.2, HBD=0 → strong BBB, might be too insoluble
6. z002015 - clogP=4.14, TPSA=23.6, HBD=0 → strong BBB, might be too insoluble
7. z002746 - clogP=1.55, TPSA=30.2, HBD=0 → BBB likely, might be too soluble
8. z002040 - clogP=1.77, TPSA=46.2, HBD=1 → possible

### Unlikely to qualify:
- z000889: clogP=8.84 → way too insoluble
- z001510: clogP=-1.04, TPSA=72.7 → too soluble, poor BBB
- z001543: clogP=-1.64, TPSA=119.6 → too soluble, no BBB
- z002216: TPSA=98.0 → poor BBB
- z002683: TPSA=100.7 → poor BBB
- z002730: TPSA=83.6, clogP=0.09 → poor BBB
- z015334: TPSA=81.6 → poor BBB
- z000364: TPSA=83.1 → poor BBB

## Assay Results (all 8 used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002170 | -2.92 | 1 | YES ✓ |
| z003001 | -0.137 | 1 | No (logS > -2) |
| z002010 | -5.0097 | 1 | No (logS << -3) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002015 | -3.82 | 1 | No (logS < -3) |
| z002746 | -0.6286 | 1 | No (logS > -2) |
| z002040 | -2.3374 | 1 | YES ✓ |

## Conclusions
- 2 qualifying compounds found: z002170 and z002040
- All 8 tested compounds had bbb=1 (confirming BBB prediction from descriptors)
- The logS sweet spot (-3 to -2) was narrow; most BBB-penetrant compounds were either too soluble (clogP < 2) or too insoluble (clogP > 4)
- z002170 (clogP=3.0, TPSA=34.9) hit the sweet spot perfectly
- z002040 (clogP=1.77, TPSA=46.2) also landed in range
- Remaining untested compounds unlikely to qualify based on descriptors (high TPSA → no BBB, or extreme clogP → wrong solubility)
