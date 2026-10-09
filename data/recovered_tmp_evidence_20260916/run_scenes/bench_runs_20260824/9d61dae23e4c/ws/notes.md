# Notes

## Strategy
Need logS between -3 and -2 (inclusive) AND bbb=1.

For BBB: low TPSA (<70), low HBD (0-1), moderate clogP (1-3), MW <400.
For logS -3 to -2: moderate lipophilicity needed (clogP ~2-3.5).

## Compounds Skipped (by descriptor reasoning)
- z000889: clogP=8.84 - way too lipophilic, logS likely << -3
- z001510: clogP=-1.04 - too hydrophilic, logS likely > -2
- z001543: clogP=-1.64, TPSA=119.6, HBD=5 - won't cross BBB
- z002010: clogP=4.28 - too lipophilic
- z002011: clogP=4.32 - too lipophilic
- z002015: clogP=4.14 - too lipophilic
- z002891: clogP=4.72 - too lipophilic
- z002216: TPSA=98.0 - unlikely BBB
- z002683: TPSA=100.7 - unlikely BBB
- z002730: clogP=0.09 - too hydrophilic
- z002764: clogP=3.56, HBD=2 - borderline
- z015334: clogP=0.74, TPSA=81.6 - borderline

## Assay Results (8/8 used)

| mol_id  | logS    | bbb | Qualifies? |
|---------|---------|-----|------------|
| z001999 | -1.522  | 1   | No (logS > -2) |
| z002746 | -0.629  | 1   | No (logS > -2) |
| z003001 | -0.137  | 1   | No (logS > -2) |
| z002040 | -2.337  | 1   | YES ✓ |
| z002170 | -2.920  | 1   | YES ✓ |
| z002003 | -3.600  | 1   | No (logS < -3) |
| z002052 | -2.789  | 1   | YES ✓ |
| z000364 | -0.948  | 0   | No (bbb=0) |

## Qualifying Compounds
- z002040: logS=-2.337, bbb=1
- z002170: logS=-2.920, bbb=1
- z002052: logS=-2.789, bbb=1
