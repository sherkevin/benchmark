# Notes

## Task
Find compounds with logS in [-3, -2] (inclusive) AND bbb=1. Budget: 8 assays out of 20 compounds.

## Strategy
For BBB penetration: need low TPSA (<60-70), low HBD (0-1), moderate clogP (1-4)
For logS in [-3,-2]: need moderate lipophilicity

## Top 8 selected for assay:
1. z003001 (clogP=2.9, TPSA=29.1, HBD=1)
2. z002746 (clogP=1.55, TPSA=30.2, HBD=0)
3. z002170 (clogP=3.0, TPSA=34.9, HBD=0)
4. z001999 (clogP=1.5, TPSA=41.9, HBD=1)
5. z002040 (clogP=1.77, TPSA=46.2, HBD=1)
6. z002003 (clogP=3.58, TPSA=43.1, HBD=0)
7. z002052 (clogP=1.99, TPSA=65.1, HBD=0)
8. z002010 (clogP=4.28, TPSA=29.5, HBD=0)

## Assay Results

| mol_id  | logS     | bbb | Qualifies? |
|---------|----------|-----|------------|
| z003001 | -0.137   | 1   | No (logS too high, too soluble) |
| z002746 | -0.629   | 1   | No (logS too high, too soluble) |
| z002170 | -2.92    | 1   | YES ✓ (logS in [-3,-2], bbb=1) |
| z001999 | -1.522   | 1   | No (logS too high, too soluble) |
| z002040 | -2.337   | 1   | YES ✓ (logS in [-3,-2], bbb=1) |
| z002003 | -3.6     | 1   | No (logS too low, too insoluble) |
| z002052 | -2.789   | 1   | YES ✓ (logS in [-3,-2], bbb=1) |
| z002010 | -5.01    | 1   | No (logS too low, too insoluble) |

## Conclusion
3 compounds qualify: z002170, z002040, z002052

Interesting observations:
- All 8 assayed compounds had bbb=1 (our BBB filtering by TPSA/HBD worked well)
- The main discriminator was solubility: low clogP compounds (z003001, z002746, z001999) were too soluble (logS > -2), while high clogP compounds (z002003, z002010) were too insoluble (logS < -3)
- The sweet spot was clogP ~1.8-3.0 with moderate TPSA
- Budget fully used: 8/8 assays
