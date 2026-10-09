# Notes

## Task
Find all compounds with measured logS between -3 and -2 (inclusive) AND measured bbb=1.
Budget: 8 assays out of 20 compounds.

## Compound Analysis

### Compounds likely to cross BBB (low TPSA < 70, low HBD ≤ 2, moderate clogp):
- z001999: clogp=1.5, TPSA=41.9, HBD=1 — good balance
- z002003: clogp=3.58, TPSA=43.1, HBD=0 — good BBB, maybe borderline solubility
- z002010: clogp=4.28, TPSA=29.5, HBD=0 — BBB likely, maybe too insoluble
- z002011: clogp=4.32, TPSA=30.2, HBD=0 — BBB likely, maybe too insoluble
- z002015: clogp=4.14, TPSA=23.6, HBD=0 — BBB likely, maybe too insoluble
- z002040: clogp=1.77, TPSA=46.2, HBD=1 — good balance
- z002170: clogp=3.0, TPSA=34.9, HBD=0 — excellent candidate
- z002746: clogp=1.55, TPSA=30.2, HBD=0 — good BBB, maybe too soluble
- z002891: clogp=4.72, TPSA=34.1, HBD=0 — BBB likely, maybe too insoluble
- z003001: clogp=2.9, TPSA=29.1, HBD=1 — excellent candidate
- z002052: clogp=1.99, TPSA=65.1, HBD=0 — possible
- z000889: clogp=8.84, TPSA=29.5, HBD=1 — BBB likely but logS way too low

### Compounds unlikely to cross BBB (high TPSA or high HBD):
- z000364: TPSA=83.1, HBD=1 — borderline
- z001510: clogp=-1.04, TPSA=72.7 — too hydrophilic, probably too soluble
- z001543: TPSA=119.6, HBD=5 — definitely no BBB
- z002216: TPSA=98.0, HBD=2 — unlikely BBB
- z002683: TPSA=100.7, HBD=2 — unlikely BBB
- z002730: clogp=0.09, TPSA=83.6 — probably too soluble, borderline BBB
- z002764: TPSA=66.8, HBD=2 — possible but less likely
- z015334: clogp=0.74, TPSA=81.6 — probably too soluble

## Strategy
Pick 8 most likely to satisfy BOTH criteria (logS -3 to -2 AND bbb=1).
Sweet spot: clogp ~1.5-3.5 for logS, and TPSA < 70, HBD ≤ 2 for BBB.

## Selected for Assays (8 total)
1. z003001 (clogp=2.9, TPSA=29.1) — top pick
2. z002170 (clogp=3.0, TPSA=34.9) — top pick
3. z001999 (clogp=1.5, TPSA=41.9) — good balance
4. z002040 (clogp=1.77, TPSA=46.2) — good balance
5. z002003 (clogp=3.58, TPSA=43.1) — good BBB, moderate solubility
6. z002746 (clogp=1.55, TPSA=30.2) — good BBB, maybe too soluble
7. z002052 (clogp=1.99, TPSA=65.1) — moderate
8. z000364 (clogp=2.87, TPSA=83.1) — borderline TPSA but decent otherwise

## Assay Results (all 8 used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z003001 | -0.137 | 1 | No (logS > -2) |
| z002170 | -2.92 | 1 | **YES** ✓ |
| z001999 | -1.522 | 1 | No (logS > -2) |
| z002040 | -2.3374 | 1 | **YES** ✓ |
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002746 | -0.6286 | 1 | No (logS > -2) |
| z002052 | -2.7894 | 1 | **YES** ✓ |
| z000364 | -0.9482 | 0 | No (bbb=0) |

## Qualifying Compounds
- **z002170**: logS=-2.92, bbb=1
- **z002040**: logS=-2.3374, bbb=1
- **z002052**: logS=-2.7894, bbb=1

## Untested Compounds Assessment
The remaining 12 compounds were not tested because:
- z000889 (clogp=8.84): Too lipophilic, logS almost certainly < -3
- z001510 (clogp=-1.04): Too hydrophilic, logS almost certainly > -2
- z001543 (TPSA=119.6, HBD=5): Cannot cross BBB
- z002010 (clogp=4.28): Too lipophilic, logS likely < -3 (similar to z002003 at clogp=3.58 → logS=-3.6)
- z002011 (clogp=4.32): Same reasoning as z002010
- z002015 (clogp=4.14): Same reasoning as z002010
- z002216 (TPSA=98.0): Unlikely to cross BBB
- z002683 (TPSA=100.7): Unlikely to cross BBB
- z002730 (clogp=0.09): Too hydrophilic, logS likely > -2
- z002764 (clogp=3.56, TPSA=66.8): Similar clogp to z002003 (logS=-3.6), likely too insoluble
- z002891 (clogp=4.72): Too lipophilic, logS likely < -3
- z015334 (clogp=0.74, TPSA=81.6): Too hydrophilic for target logS range, borderline BBB
