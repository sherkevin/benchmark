# Notes

## Task
Identify compounds with logS between -3 and -2 (inclusive) AND bbb=1 from 20 candidates. Budget: 8 assays.

## Analysis of Candidates

For BBB penetration, need: low TPSA (<70), low HBD (0-1), moderate clogp (2-5).
For logS in [-3,-2]: moderate clogp (~2-4), not too lipophilic or hydrophilic.

### Compounds ruled out by descriptors:
- z000889: clogp=8.84 - way too lipophilic, logS will be << -3
- z001510: clogp=-1.04, TPSA=72.7 - too hydrophilic, too polar
- z001543: clogp=-1.64, TPSA=119.6, HBD=5 - very polar, no BBB chance
- z002216: clogp=0.86, TPSA=98.0, HBD=2 - too polar for BBB
- z002683: TPSA=100.7 - too polar for BBB
- z002730: clogp=0.09, TPSA=83.6 - too polar, too soluble
- z015334: clogp=0.74, TPSA=81.6 - too polar

### Borderline/unlikely:
- z000364: TPSA=83.1 - too high for BBB
- z002040: clogp=1.77 - might be too soluble
- z002764: TPSA=66.8, HBD=2 - borderline

### Top candidates to assay (8 picks):
1. z002003: clogp=3.58, TPSA=43.1, HBD=0 → great BBB profile, moderate solubility
2. z002170: clogp=3.0, TPSA=34.9, HBD=0 → excellent balance
3. z003001: clogp=2.9, TPSA=29.1, HBD=1 → good balance
4. z002015: clogp=4.14, TPSA=23.6, HBD=0 → strong BBB, solubility borderline
5. z002010: clogp=4.28, TPSA=29.5, HBD=0 → strong BBB, solubility borderline
6. z002011: clogp=4.32, TPSA=30.2, HBD=0 → strong BBB, solubility borderline
7. z002746: clogp=1.55, TPSA=30.2, HBD=0 → good BBB, might be too soluble
8. z001999: clogp=1.5, TPSA=41.9, HBD=1 → possible but less certain

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002170 | -2.92 | 1 | **YES** ✓ |
| z003001 | -0.137 | 1 | NO (logS > -2) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002010 | -5.01 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002746 | -0.63 | 1 | NO (logS > -2) |
| z001999 | -1.52 | 1 | NO (logS > -2) |

## Summary
- All 8 tested compounds had bbb=1 (good BBB profiles based on descriptors)
- Solubility was the key discriminator
- Compounds with clogp ~4+ had logS < -3 (too insoluble)
- Compounds with clogp ~1.5 had logS > -2 (too soluble)
- Only z002170 (clogp=3.0, TPSA=34.9, HBD=0) fell in the sweet spot with logS=-2.92
- Budget exhausted after 8 assays

## Remaining untested compounds analysis:
- z000364 (clogp=2.87, TPSA=83.1): TPSA too high for BBB
- z000889 (clogp=8.84): far too insoluble
- z001510, z001543, z002730: too hydrophilic
- z002216, z002683, z015334: too polar for BBB
- z002040 (clogp=1.77): likely too soluble
- z002052 (clogp=1.99, TPSA=65.1): possible but borderline
- z002764 (clogp=3.56, HBD=2): HBD=2 hurts BBB
- z002891 (clogp=4.72): likely too insoluble
