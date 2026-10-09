# Notes

## Task
Find qualifying compounds from 20 candidates where:
- logS between -3 and -2 (inclusive)
- bbb = 1
Budget: 8 assays total

## Analysis of candidates
Prioritizing compounds with:
- Low TPSA (<70) for BBB penetration
- Moderate clogp (1-4) for both BBB and moderate solubility
- Low MW, few HBD for BBB
- Not too lipophilic (clogp < 4) for reasonable solubility

Top 8 picks for assay:
1. z002746 - clogp=1.55, TPSA=30.2, MW=231 (great BBB profile)
2. z001999 - clogp=1.5, TPSA=41.9, MW=299 (great BBB profile)
3. z002040 - clogp=1.77, TPSA=46.2, MW=217 (great BBB profile)
4. z003001 - clogp=2.9, TPSA=29.1, MW=238 (great BBB, moderate clogp)
5. z002170 - clogp=3.0, TPSA=34.9, MW=250 (good BBB profile)
6. z002003 - clogp=3.58, TPSA=43.1, MW=309 (good BBB, higher clogp)
7. z002052 - clogp=1.99, TPSA=65.1, MW=369 (borderline BBB)
8. z000364 - clogp=2.87, TPSA=83.1, MW=399 (borderline BBB)

Compounds skipped (likely won't qualify):
- z000889: clogp=8.84 → too lipophilic, poor solubility
- z001510: clogp=-1.04 → too hydrophilic, too soluble
- z001543: TPSA=119.6 → poor BBB
- z002010: clogp=4.28 → likely poor solubility
- z002011: clogp=4.32 → likely poor solubility
- z002015: clogp=4.14 → likely poor solubility
- z002216: TPSA=98.0 → poor BBB
- z002683: TPSA=100.7 → poor BBB
- z002730: clogp=0.09, TPSA=83.6 → poor BBB, too soluble
- z002764: TPSA=66.8, HBD=2 → borderline
- z002891: clogp=4.72 → likely poor solubility
- z015334: TPSA=81.6, clogp=0.74 → poor BBB

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002746 | -0.6286 | 1 | No - logS too high (> -2) |
| z001999 | -1.522 | 1 | No - logS too high (> -2) |
| z002040 | -2.3374 | 1 | YES - logS in [-3,-2], bbb=1 |
| z003001 | -0.137 | 1 | No - logS too high (> -2) |
| z002170 | -2.92 | 1 | YES - logS in [-3,-2], bbb=1 |
| z002003 | -3.6 | 1 | No - logS too low (< -3) |
| z002052 | -2.7894 | 1 | YES - logS in [-3,-2], bbb=1 |
| z000364 | -0.9482 | 0 | No - bbb=0 |

## Conclusion
3 compounds qualify: z002040, z002170, z002052

Observations:
- Compounds with clogp ~1.5-2.0 were too soluble (logS > -2)
- Compounds with clogp ~3.0-3.58 were in the sweet spot or slightly too insoluble
- BBB was easy to achieve (7/8 had bbb=1) among my selected low-TPSA compounds
- z000364 with TPSA=83.1 was the only one with bbb=0
- Untested compounds with very high clogp (>4) likely too insoluble
- Untested compounds with high TPSA (>80) likely won't penetrate BBB
- z002764 (clogp=3.56, TPSA=66.8) was the most likely missed candidate but with HBD=2 and similar clogp to z002003 (logS=-3.6), it would likely fall just outside the range
