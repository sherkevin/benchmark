# Notes

## Task
Identify compounds with logS between -3 and -2 (inclusive) AND bbb = 1.
Budget: 8 assays out of 20 compounds.

## Strategy
For BBB penetration: need low TPSA (<60, ideally <50), moderate clogp (1-4), MW < 450, low HBD.
For logS in [-3, -2]: moderate lipophilicity needed. Sweet spot: clogp ~2-3.5 based on results.
Selected 8 compounds most likely to satisfy both criteria based on descriptor analysis.

## Compounds Tested (8/8 assays used)

| mol_id  | clogp | tpsa | logS    | bbb | Qualifies? |
|---------|-------|------|---------|-----|------------|
| z002170 | 3.0   | 34.9 | -2.92   | 1   | ✅ YES     |
| z002040 | 1.77  | 46.2 | -2.3374 | 1   | ✅ YES     |
| z003001 | 2.9   | 29.1 | -0.137  | 1   | ❌ logS too high (>-2) |
| z002746 | 1.55  | 30.2 | -0.6286 | 1   | ❌ logS too high (>-2) |
| z002003 | 3.58  | 43.1 | -3.6    | 1   | ❌ logS too low (<-3) |
| z001999 | 1.5   | 41.9 | -1.522  | 1   | ❌ logS too high (>-2) |
| z002010 | 4.28  | 29.5 | -5.0097 | 1   | ❌ logS way too low |
| z002015 | 4.14  | 23.6 | -3.82   | 1   | ❌ logS too low (<-3) |

## Key Observations
- All 8 tested compounds had bbb=1 (our BBB selection criteria worked well)
- The logS window [-3, -2] is narrow; clogp ~2-3 seems optimal
- clogp < 2 → too soluble (logS > -2): z003001, z002746, z001999
- clogp > 3.5 → too insoluble (logS < -3): z002003, z002010, z002015
- clogp ~2-3 hits the sweet spot: z002170 (3.0), z002040 (1.77 but larger MW helps)

## Untested Compounds (and why excluded)
- z000364: TPSA 83.1 → poor BBB expected
- z000889: clogp 8.84 → far too insoluble
- z001510: clogp -1.04 → far too soluble
- z001543: clogp -1.64, TPSA 119.6 → too soluble, no BBB
- z002011: clogp 4.32 → likely too insoluble (like z002010/z002015)
- z002052: clogp 1.99, TPSA 65.1 → borderline BBB, possible candidate
- z002216: TPSA 98.0 → poor BBB
- z002683: TPSA 100.7 → poor BBB
- z002730: clogp 0.09, TPSA 83.6 → too soluble, poor BBB
- z002764: clogp 3.56, TPSA 66.8, HBD 2 → borderline BBB, likely too insoluble
- z002891: clogp 4.72 → likely too insoluble
- z015334: clogp 0.74, TPSA 81.6 → too soluble, poor BBB

## Conclusion
Qualifying compounds confirmed by assay: z002170, z002040
