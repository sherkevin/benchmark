# Medicinal Chemistry Triaging Notes

## Task
- 20 compounds, budget of 8 assays
- Qualify: logS between [-3, -2] AND bbb=1

## Descriptor Analysis

Compounds ruled OUT (unlikely to qualify):
- z000889: clogp=8.84 → far too lipophilic, logS will be << -3
- z001510: clogp=-1.04, TPSA=72.7 → too soluble, poor BBB
- z001543: clogp=-1.64, TPSA=119.6 → too soluble, poor BBB
- z002010: clogp=4.28 → likely too insoluble
- z002011: clogp=4.32 → likely too insoluble
- z002015: clogp=4.14 → likely too insoluble
- z002216: TPSA=98.0 → poor BBB
- z002683: TPSA=100.7 → poor BBB
- z002730: clogp=0.09, TPSA=83.6 → too soluble, poor BBB
- z002891: clogp=4.72 → likely too insoluble
- z015334: clogp=0.74, TPSA=81.6 → likely too soluble, poor BBB

Top candidates (moderate clogp, low TPSA → could have both BBB=1 and logS in [-3,-2]):
1. z001999: clogp=1.5, TPSA=41.9
2. z002040: clogp=1.77, TPSA=46.2
3. z002746: clogp=1.55, TPSA=30.2
4. z003001: clogp=2.9, TPSA=29.1
5. z002170: clogp=3.0, TPSA=34.9
6. z002003: clogp=3.58, TPSA=43.1
7. z002052: clogp=1.99, TPSA=65.1
8. z002764: clogp=3.56, TPSA=66.8

Borderline (won't test unless budget allows):
- z000364: clogp=2.87, TPSA=83.1 (TPSA a bit high)

## Assay Results (8/8 used)

| mol_id  | logS     | bbb | Qualifies? |
|---------|----------|-----|------------|
| z001999 | -1.522   | 1   | NO (logS > -2, too soluble) |
| z002040 | -2.3374  | 1   | YES ✓ |
| z002746 | -0.6286  | 1   | NO (logS > -2, too soluble) |
| z003001 | -0.137   | 1   | NO (logS > -2, too soluble) |
| z002170 | -2.92    | 1   | YES ✓ |
| z002003 | -3.6     | 1   | NO (logS < -3, too insoluble) |
| z002052 | -2.7894  | 1   | YES ✓ |
| z002764 | -2.9008  | 0   | NO (bbb=0) |

## Qualifying Compounds (confirmed by assay):
1. z002040 (logS=-2.3374, bbb=1)
2. z002170 (logS=-2.92, bbb=1)
3. z002052 (logS=-2.7894, bbb=1)

## Untested compounds - reasoning for exclusion:
- z000889 (clogp=8.84): Far too lipophilic, logS certainly << -3
- z001510 (clogp=-1.04, TPSA=72.7): Too soluble (logS > -2) and poor BBB
- z001543 (clogp=-1.64, TPSA=119.6): Too soluble and poor BBB
- z002010 (clogp=4.28): More lipophilic than z002003 (logS=-3.6), likely too insoluble
- z002011 (clogp=4.32): Same reasoning, likely too insoluble
- z002015 (clogp=4.14): Same reasoning, likely too insoluble
- z002216 (TPSA=98.0): High TPSA → poor BBB
- z002683 (TPSA=100.7): High TPSA → poor BBB
- z002730 (clogp=0.09, TPSA=83.6): Too soluble, poor BBB
- z002891 (clogp=4.72): Very lipophilic, likely too insoluble
- z015334 (clogp=0.74, TPSA=81.6): Likely too soluble, poor BBB
- z000364 (clogp=2.87, TPSA=83.1): TPSA too high for BBB (z002764 TPSA=66.8 got bbb=0)
