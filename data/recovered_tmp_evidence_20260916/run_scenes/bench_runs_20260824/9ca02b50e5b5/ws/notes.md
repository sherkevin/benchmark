# Notes

## Task
Find compounds with logS between -3 and -2 (inclusive) AND bbb=1. Budget: 8 assays for 20 compounds.

## Descriptor Analysis

Compounds most likely to qualify need:
- Moderate clogp (~1.5-3.5) for logS in range
- Low TPSA (<70) and low HBD for BBB penetration

### Top candidates selected for assay:
1. z002746: clogp=1.55, TPSA=30.2, HBD=0 — excellent BBB profile
2. z003001: clogp=2.9, TPSA=29.1, HBD=1 — good BBB, good logP
3. z002170: clogp=3.0, TPSA=34.9, HBD=0 — good BBB, moderate logP
4. z002040: clogp=1.77, TPSA=46.2, HBD=1 — small, good profile
5. z001999: clogp=1.5, TPSA=41.9, HBD=1 — good profile
6. z002052: clogp=1.99, TPSA=65.1, HBD=0 — moderate TPSA
7. z002003: clogp=3.58, TPSA=43.1, HBD=0 — higher logP but good BBB
8. z000364: clogp=2.87, TPSA=83.1, HBD=1 — moderate TPSA, good logP

### Compounds excluded (and why):
- z000889: clogp=8.84 — far too lipophilic
- z001510: clogp=-1.04 — too hydrophilic
- z001543: clogp=-1.64, TPSA=119.6 — too hydrophilic and poor BBB
- z002010: clogp=4.28 — likely too insoluble
- z002011: clogp=4.32 — likely too insoluble
- z002015: clogp=4.14 — likely too insoluble
- z002216: TPSA=98.0 — poor BBB
- z002683: TPSA=100.7 — poor BBB
- z002730: clogp=0.09 — likely too soluble
- z002764: clogp=3.56, TPSA=66.8, HBD=2 — borderline
- z002891: clogp=4.72 — likely too insoluble
- z015334: clogp=0.74, TPSA=81.6 — likely too soluble

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|-----------|
| z002746 | -0.6286 | 1 | No — logS > -2 (too soluble) |
| z003001 | -0.137 | 1 | No — logS > -2 (too soluble) |
| z002170 | -2.92 | 1 | **YES** — logS in [-3,-2], bbb=1 |
| z002040 | -2.3374 | 1 | **YES** — logS in [-3,-2], bbb=1 |
| z001999 | -1.522 | 1 | No — logS > -2 (too soluble) |
| z002052 | -2.7894 | 1 | **YES** — logS in [-3,-2], bbb=1 |
| z002003 | -3.6 | 1 | No — logS < -3 (too insoluble) |
| z000364 | -0.9482 | 0 | No — bbb=0 and logS > -2 |

## Conclusion
3 qualifying compounds confirmed: z002170, z002040, z002052.
All 8 assay budget units used.

### Untested compounds rationale:
Remaining 12 compounds were excluded based on descriptors suggesting they would not meet both criteria:
- Very high clogp (>4): z000889, z002010, z002011, z002015, z002891 — likely logS < -3
- Very low clogp (<1): z001510, z001543, z002730, z015334 — likely logS > -2
- High TPSA (>90): z002216, z002683 — likely bbb=0
- z002764 (clogp=3.56, TPSA=66.8, HBD=2) was borderline but budget was exhausted
