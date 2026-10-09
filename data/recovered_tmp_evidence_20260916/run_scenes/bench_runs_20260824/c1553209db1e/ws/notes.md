# Notes

## Strategy
- Need logS in [-3, -2] and bbb = 1
- For logS in range: clogP roughly 1.5-4.0 (not too lipophilic, not too hydrophilic)
- For BBB: TPSA < ~80, moderate clogP, low HBD

## Compound Analysis (sorted by promise)
| mol_id  | clogP | TPSA | HBD | Assessment                          |
|---------|-------|------|-----|-------------------------------------|
| z002170 | 3.0   | 34.9 | 0   | Ideal clogP, low TPSA - TOP PICK    |
| z003001 | 2.9   | 29.1 | 1   | Ideal clogP, low TPSA - TOP PICK    |
| z002003 | 3.58  | 43.1 | 0   | Good clogP, low TPSA                |
| z002746 | 1.55  | 30.2 | 0   | Might be too soluble, but low TPSA  |
| z002040 | 1.77  | 46.2 | 1   | Moderate clogP, low TPSA            |
| z002052 | 1.99  | 65.1 | 0   | Good clogP, TPSA borderline         |
| z001999 | 1.5   | 41.9 | 1   | Low clogP, might be too soluble     |
| z002764 | 3.56  | 66.8 | 2   | Good clogP, TPSA borderline         |

## Excluded (and why)
- z000889: clogP=8.84 - way too insoluble
- z001510: clogP=-1.04 - way too soluble
- z001543: clogP=-1.64, TPSA=119.6 - too soluble, no BBB
- z002010: clogP=4.28 - probably too insoluble
- z002011: clogP=4.32 - probably too insoluble
- z002015: clogP=4.14 - probably too insoluble
- z002216: TPSA=98 - probably no BBB
- z002683: TPSA=100.7 - probably no BBB
- z002730: clogP=0.09 - too soluble
- z002891: clogP=4.72 - probably too insoluble
- z000364: TPSA=83.1, clogP=2.87 - possible but TPSA borderline
- z015334: clogP=0.74, TPSA=81.6 - too soluble, TPSA borderline

## Assay Results
| mol_id  | logS     | bbb | Qualifies? |
|---------|----------|-----|------------|
| z002170 | -2.92    | 1   | YES        |
| z003001 | -0.137   | 1   | NO (too soluble) |
| z002003 | -3.6     | 1   | NO (too insoluble) |
| z002746 | -0.629   | 1   | NO (too soluble) |
| z002040 | -2.337   | 1   | YES        |
| z002052 | -2.789   | 1   | YES        |
| z001999 | -1.522   | 1   | NO (too soluble) |
| z002764 | -2.901   | 0   | NO (no BBB) |

## Final Answer
Qualifying compounds: z002170, z002040, z002052
All 8 assays used.
