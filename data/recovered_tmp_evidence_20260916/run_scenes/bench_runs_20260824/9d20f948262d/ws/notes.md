# Notes on Compound Screening

## Target Profile
- logS between -3 and -2 (inclusive)
- bbb = 1

## Budget: 8 assays

## Strategy
Looking for compounds with:
- TPSA < 90 (for BBB penetration)
- Moderate clogP (not extreme)
- Low HBD count

## Candidates Analysis

| mol_id | clogP | TPSA | HBD | Notes |
|--------|-------|------|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | Good candidate |
| z000889 | 8.84 | 29.5 | 1 | clogP too high, poor solubility likely |
| z001510 | -1.04 | 72.7 | 1 | Low clogP, may have good solubility |
| z001543 | -1.64 | 119.6 | 5 | TPSA too high, HBD too high - skip |
| z001999 | 1.5 | 41.9 | 1 | Good candidate |
| z002003 | 3.58 | 43.1 | 0 | Good candidate |
| z002010 | 4.28 | 29.5 | 0 | Good candidate |
| z002011 | 4.32 | 30.2 | 0 | Good candidate |
| z002015 | 4.14 | 23.6 | 0 | Good candidate |
| z002040 | 1.77 | 46.2 | 1 | Good candidate |
| z002052 | 1.99 | 65.1 | 0 | Good candidate |
| z002170 | 3.0 | 34.9 | 0 | Good candidate |
| z002216 | 0.86 | 98.0 | 2 | TPSA high - skip |
| z002683 | 2.55 | 100.7 | 2 | TPSA high - skip |
| z002730 | 0.09 | 83.6 | 1 | Low clogP, may work |
| z002746 | 1.55 | 30.2 | 0 | Good candidate |
| z002764 | 3.56 | 66.8 | 2 | Good candidate |
| z002891 | 4.72 | 34.1 | 0 | Good candidate |
| z003001 | 2.9 | 29.1 | 1 | Good candidate |
| z015334 | 0.74 | 81.6 | 1 | Good candidate |

## Assays Run

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002040 | -2.3374 | 1 | YES |
| z002170 | -2.92 | 1 | YES |
| z002746 | -0.6286 | 1 | NO (logS > -2) |
| z003001 | -0.137 | 1 | NO (logS > -2) |

## Results

**Qualifying compounds:** z002040, z002170

Both have:
- logS between -3 and -2 (inclusive)
- bbb = 1
