# Notes - Compound Triage

## Task
Find compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays maximum

## Strategy
Looking at the descriptors to prioritize compounds likely to meet criteria:

### BBB penetration factors:
- TPSA < 90 (ideally < 70)
- Moderate clogP (2-4 range)
- Low HBD count

### Solubility factors (logS -3 to -2):
- This is moderate solubility range
- clogP around 2-4 might be ideal
- Not too polar, not too lipophilic

## Compound Analysis

| mol_id | mw | clogp | tpsa | hbd | hba | rotb | rings | Notes |
|--------|-----|-------|------|-----|-----|------|-------|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 5 | 3 | Moderate clogP, TPSA ok |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 12 | 2 | Too lipophilic, poor solubility likely |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 0 | 2 | Very polar, low clogP - might have good solubility but BBB? |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 4 | 2 | Too polar, high HBD - poor BBB |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 1 | 5 | Good profile for BBB, moderate clogP |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 1 | 4 | Good BBB profile, clogP in range |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 8 | 2 | Higher clogP, might be too lipophilic |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 1 | 4 | Similar to above |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 6 | 3 | High clogP, low TPSA |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | 2 | Good balance |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 2 | 5 | Good balance |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 1 | 3 | Good profile |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 3 | 2 | High TPSA - poor BBB likely |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 3 | 2 | High TPSA - poor BBB likely |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 1 | 3 | Low clogP, might be too soluble |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | 2 | Good BBB profile |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 2 | 4 | Good balance |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 1 | 4 | High clogP - poor solubility likely |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | 2 | Good profile |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 5 | 3 | Lower clogP, higher TPSA |

## Priority Selection
Best candidates for BBB=1 AND logS in [-3,-2]:
1. z001999 - clogP 1.5, TPSA 41.9
2. z002003 - clogP 3.58, TPSA 43.1
3. z002040 - clogP 1.77, TPSA 46.2
4. z002052 - clogP 1.99, TPSA 65.1
5. z002170 - clogP 3.0, TPSA 34.9
6. z002746 - clogP 1.55, TPSA 30.2
7. z002764 - clogP 3.56, TPSA 66.8
8. z003001 - clogP 2.9, TPSA 29.1

## Assays Run

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO - logS > -2 |
| z002003 | -3.6 | 1 | NO - logS < -3 |
| z002040 | -2.3374 | 1 | YES |
| z002052 | -2.7894 | 1 | YES |
| z002170 | -2.92 | 1 | YES |
| z002746 | -0.6286 | 1 | NO - logS > -2 |
| z002764 | -2.9008 | 0 | NO - bbb=0 |
| z003001 | -0.137 | 1 | NO - logS > -2 |

## Qualifying Compounds
- z002040 (logS=-2.3374, bbb=1)
- z002052 (logS=-2.7894, bbb=1)
- z002170 (logS=-2.92, bbb=1)

Budget used: 8/8
