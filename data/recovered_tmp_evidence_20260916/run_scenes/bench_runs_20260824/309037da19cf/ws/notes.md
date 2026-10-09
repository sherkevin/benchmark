# Notes

## Task
Find compounds with logS between -3 and -2 (inclusive) AND bbb=1. Budget: 8 assays total.

## Strategy
Screened 20 compounds by descriptors. Prioritized those with:
- clogp ~2-4 (for moderate solubility, logS in target range)
- TPSA < 60 (for BBB penetration)
- HBD ≤ 1 (for BBB penetration)
- MW < 400

## 8 Assays Performed

| mol_id | clogp | TPSA | logS | bbb | Qualifies? |
|--------|-------|------|------|-----|-----------|
| z003001 | 2.9 | 29.1 | -0.137 | 1 | NO (too soluble) |
| z002170 | 3.0 | 34.9 | -2.92 | 1 | **YES** |
| z002003 | 3.58 | 43.1 | -3.6 | 1 | NO (too insoluble) |
| z002746 | 1.55 | 30.2 | -0.629 | 1 | NO (too soluble) |
| z001999 | 1.5 | 41.9 | -1.522 | 1 | NO (too soluble) |
| z002040 | 1.77 | 46.2 | -2.337 | 1 | **YES** |
| z002010 | 4.28 | 29.5 | -5.010 | 1 | NO (too insoluble) |
| z002011 | 4.32 | 30.2 | -4.13 | 1 | NO (too insoluble) |

## Key Observations
- All 8 tested compounds had bbb=1 (our BBB filtering by TPSA/HBD worked well)
- The solubility sweet spot was clogp ~2-3 for this dataset
- clogp < 2 → logS > -2 (too soluble)
- clogp ~2-3 → logS in [-3, -2] (target range)
- clogp > 3.5 → logS < -3 (too insoluble)

## Untested compounds with clogp in sweet spot (2-3.5):
- z000364: clogp=2.87 but TPSA=83.1 (likely no BBB)
- z002052: clogp=1.99, TPSA=65.1 (borderline BBB)
- z002683: clogp=2.55, TPSA=100.7 (likely no BBB)
- z002764: clogp=3.56, TPSA=66.8, HBD=2 (borderline BBB)

Budget exhausted - cannot test more.

## Qualifying Compounds
- z002170 (logS=-2.92, bbb=1)
- z002040 (logS=-2.337, bbb=1)
