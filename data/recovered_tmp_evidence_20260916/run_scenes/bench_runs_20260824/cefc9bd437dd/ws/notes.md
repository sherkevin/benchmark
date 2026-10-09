# Notes - Compound Triage

## Task
Find compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays max

## Strategy
Looking at descriptors to prioritize compounds likely to have:
- Good brain penetration (bbb=1): TPSA < 90, moderate clogP, low HBD
- Moderate solubility (logS -3 to -2): not too hydrophilic or lipophilic

## Candidates Analysis

| mol_id | clogP | TPSA | HBD | Notes |
|--------|-------|------|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | Moderate clogP, TPSA ok |
| z000889 | 8.84 | 29.5 | 1 | Very high clogP - likely poor solubility |
| z001510 | -1.04 | 72.7 | 1 | Low clogP - may be too soluble |
| z001543 | -1.64 | 119.6 | 5 | High TPSA, high HBD - poor BBB |
| z001999 | 1.5 | 41.9 | 1 | Good profile for BBB |
| z002003 | 3.58 | 43.1 | 0 | Good profile for BBB |
| z002010 | 4.28 | 29.5 | 0 | Good BBB potential |
| z002011 | 4.32 | 30.2 | 0 | Good BBB potential |
| z002015 | 4.14 | 23.6 | 0 | Good BBB potential |
| z002040 | 1.77 | 46.2 | 1 | Moderate profile |
| z002052 | 1.99 | 65.1 | 0 | Moderate profile |
| z002170 | 3.0 | 34.9 | 0 | Good BBB potential |
| z002216 | 0.86 | 98.0 | 2 | High TPSA - poor BBB |
| z002683 | 2.55 | 100.7 | 2 | High TPSA - poor BBB |
| z002730 | 0.09 | 83.6 | 1 | Low clogP - may be too soluble |
| z002746 | 1.55 | 30.2 | 0 | Good BBB potential |
| z002764 | 3.56 | 66.8 | 2 | Moderate profile |
| z002891 | 4.72 | 34.1 | 0 | High clogP - may have low solubility |
| z003001 | 2.9 | 29.1 | 1 | Good BBB potential |
| z015334 | 0.74 | 81.6 | 1 | Moderate profile |

## Assays Run

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | No - logS > -2 |
| z002003 | -3.6 | 1 | No - logS < -3 |
| z002010 | -5.0097 | 1 | No - logS < -3 |
| z002011 | -4.13 | 1 | No - logS < -3 |
| z002015 | -3.82 | 1 | No - logS < -3 |
| z002170 | -2.92 | 1 | **YES** |
| z002746 | -0.6286 | 1 | No - logS > -2 |
| z003001 | -0.137 | 1 | No - logS > -2 |

## Conclusion

Only **z002170** qualifies:
- logS = -2.92 (within -3 to -2 range)
- bbb = 1

Budget used: 8/8 assays
