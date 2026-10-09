# Notes

## Task
Find compounds with measured logS between -3 and -2 (inclusive) AND bbb=1.
Budget: 8 assays out of 20 compounds.

## Analysis of all 20 compounds

For BBB penetration: need low TPSA (<70), low HBD (0-1), moderate clogp (1-4)
For logS in [-3,-2]: need moderate lipophilicity, clogp ~2-4 range

### Compounds ruled out (unlikely to meet both criteria):
- z000889: clogp=8.84 → way too lipophilic, logS << -3
- z001510: clogp=-1.04, TPSA=72.7 → too soluble, poor BBB
- z001543: clogp=-1.64, TPSA=119.6 → too soluble, no BBB
- z002216: TPSA=98.0 → no BBB
- z002683: TPSA=100.7 → no BBB
- z002730: clogp=0.09, TPSA=83.6 → too soluble, no BBB
- z015334: clogp=0.74, TPSA=81.6 → too soluble, no BBB
- z002015: clogp=4.14, TPSA=23.6 → probably too lipophilic
- z002891: clogp=4.72 → very lipophilic, logS likely << -3
- z000364: TPSA=83.1 → unlikely BBB
- z002764: TPSA=66.8, HBD=2, clogp=3.56 → borderline BBB

### Top 8 candidates to assay:
1. z002170: clogp=3.0, TPSA=34.9, HBD=0 → excellent BBB profile, clogp in sweet spot
2. z003001: clogp=2.9, TPSA=29.1, HBD=1 → excellent BBB profile
3. z002746: clogp=1.55, TPSA=30.2, HBD=0 → great BBB, might be too soluble
4. z002040: clogp=1.77, TPSA=46.2, HBD=1 → good BBB, might be slightly too soluble
5. z002003: clogp=3.58, TPSA=43.1, HBD=0 → good BBB, might be slightly too lipophilic
6. z001999: clogp=1.5, TPSA=41.9, HBD=1 → good BBB, might be too soluble
7. z002052: clogp=1.99, TPSA=65.1, HBD=0 → borderline BBB TPSA, moderate clogp
8. z002010: clogp=4.28, TPSA=29.5, HBD=0 → good BBB but risky on solubility

## Assay Results

| mol_id  | logS     | bbb | Qualifies? |
|---------|----------|-----|------------|
| z002170 | -2.92    | 1   | YES ✓      |
| z003001 | -0.137   | 1   | NO (too soluble) |
| z002746 | -0.6286  | 1   | NO (too soluble) |
| z002040 | -2.3374  | 1   | YES ✓      |
| z002003 | -3.6     | 1   | NO (too insoluble) |
| z001999 | -1.522   | 1   | NO (too soluble) |
| z002052 | -2.7894  | 1   | YES ✓      |
| z002010 | -5.0097  | 1   | NO (too insoluble) |

## Summary
All 8 assays used. Found 3 qualifying compounds:
- z002170 (logS=-2.92, bbb=1)
- z002040 (logS=-2.3374, bbb=1)
- z002052 (logS=-2.7894, bbb=1)

Interestingly all 8 tested compounds penetrated BBB (bbb=1). Solubility was the key discriminator.
Compounds with clogp ~1.5 were too soluble, clogp ~3.5-4.3 were too insoluble.
The sweet spot was clogp ~1.8-3.0.
