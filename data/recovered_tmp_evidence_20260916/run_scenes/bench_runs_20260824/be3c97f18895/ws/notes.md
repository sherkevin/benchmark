# Notes

## Task
Find compounds with measured logS between -3 and -2 (inclusive) AND bbb = 1.
Budget: 8 assays out of 20 candidates.

## Analysis of candidates

For BBB penetration: need low TPSA (<60-70), low HBD (0-1), moderate clogP (1.5-4)
For moderate solubility (logS -3 to -2): need moderate clogP, not too lipophilic

### Top 8 selected for assay:
1. z002746 (MW=231.3, clogP=1.55, TPSA=30.2, HBD=0) — ideal profile
2. z003001 (MW=237.7, clogP=2.9, TPSA=29.1, HBD=1) — ideal profile
3. z002170 (MW=250.3, clogP=3.0, TPSA=34.9, HBD=0) — good profile
4. z002040 (MW=217.3, clogP=1.77, TPSA=46.2, HBD=1) — good profile
5. z001999 (MW=299.4, clogP=1.5, TPSA=41.9, HBD=1) — decent
6. z002003 (MW=308.8, clogP=3.58, TPSA=43.1, HBD=0) — might be too lipophilic for solubility
7. z002052 (MW=369.4, clogP=1.99, TPSA=65.1, HBD=0) — borderline TPSA
8. z002010 (MW=339.5, clogP=4.28, TPSA=29.5, HBD=0) — great BBB, might have poor solubility

### Compounds excluded (and why):
- z000889: clogP=8.84, way too lipophilic
- z001510: clogP=-1.04, too hydrophilic, poor BBB
- z001543: clogP=-1.64, TPSA=119.6, very poor BBB
- z002011: clogP=4.32, similar to z002010 but slightly worse
- z002015: clogP=4.14, TPSA=23.6 — good BBB but likely too insoluble
- z002216: TPSA=98.0, poor BBB
- z002683: TPSA=100.7, poor BBB
- z002730: clogP=0.09, TPSA=83.6, poor BBB
- z002764: TPSA=66.8, HBD=2, borderline
- z002891: clogP=4.72, likely too insoluble
- z015334: TPSA=81.6, clogP=0.74, poor BBB
- z000364: TPSA=83.1, poor BBB

## Assay Results

| mol_id  | logS     | bbb | Qualifies? |
|---------|----------|-----|------------|
| z002746 | -0.6286  | 1   | No (logS too high/too soluble) |
| z003001 | -0.137   | 1   | No (logS too high/too soluble) |
| z002170 | -2.92    | 1   | YES ✓ |
| z002040 | -2.3374  | 1   | YES ✓ |
| z001999 | -1.522   | 1   | No (logS too high/too soluble) |
| z002003 | -3.6     | 1   | No (logS too low/too insoluble) |
| z002052 | -2.7894  | 1   | YES ✓ |
| z002010 | -5.0097  | 1   | No (logS way too low) |

## Final Answer
3 qualifying compounds: z002170, z002040, z002052
All have bbb=1 and logS in [-3, -2] range.
