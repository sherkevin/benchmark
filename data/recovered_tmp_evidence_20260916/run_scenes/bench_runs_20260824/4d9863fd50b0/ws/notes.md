# Notes - Compound Triage

## Task Summary
- 20 compounds in candidates.tsv
- Budget: 8 assays maximum
- Qualifying criteria: logS between -3 and -2 (inclusive) AND bbb = 1

## Compound Analysis

Looking at descriptors to prioritize compounds likely to have:
- Good brain penetration (bbb=1): TPSA < 90, moderate clogP (~2-4), low HBD
- Moderate solubility (logS -3 to -2): not too hydrophobic

| mol_id | mw | clogP | TPSA | HBD | HBA | Notes |
|--------|-----|-------|------|-----|-----|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | Moderate clogP, TPSA ok |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | Very high clogP - likely poor solubility |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | Low clogP - likely high solubility (too high) |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | High TPSA, low clogP - poor BBB |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | Good profile for BBB |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | Good profile for BBB |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | Higher clogP, good for BBB |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | Higher clogP, good for BBB |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | Higher clogP, good for BBB |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | Moderate profile |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | Moderate profile |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | Good BBB candidate |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | High TPSA - poor BBB |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | High TPSA - poor BBB |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | Low clogP - maybe too soluble |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | Good BBB candidate |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | Moderate profile |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | High clogP - likely poor solubility |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | Good BBB candidate |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | Lower clogP, TPSA ok |

## Selection Strategy

I'll prioritize compounds with:
- TPSA < 70 (better brain penetration)
- clogP between 1.5 and 4.0 (balanced solubility/BBB)
- HBD <= 1

Top candidates to assay (8 max):
1. z001999 - clogP 1.5, TPSA 41.9, HBD 1
2. z002003 - clogP 3.58, TPSA 43.1, HBD 0
3. z002010 - clogP 4.28, TPSA 29.5, HBD 0
4. z002011 - clogP 4.32, TPSA 30.2, HBD 0
5. z002015 - clogP 4.14, TPSA 23.6, HBD 0
6. z002170 - clogP 3.0, TPSA 34.9, HBD 0
7. z002746 - clogP 1.55, TPSA 30.2, HBD 0
8. z003001 - clogP 2.9, TPSA 29.1, HBD 1

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002010 | -5.0097 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002170 | -2.92 | 1 | YES (-3 <= -2.92 <= -2) |
| z002746 | -0.6286 | 1 | NO (logS > -2) |
| z003001 | -0.137 | 1 | NO (logS > -2) |

## Conclusion

Only **z002170** qualifies:
- logS = -2.92 (within [-3, -2] range)
- bbb = 1

Budget used: 8/8 assays
