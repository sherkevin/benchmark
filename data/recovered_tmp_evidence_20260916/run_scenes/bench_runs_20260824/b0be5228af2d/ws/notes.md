# Compound Screening Notes

## Task Overview
- Need to find compounds with:
  1. logS between -3 and -2 (inclusive)
  2. bbb = 1
- Budget: 8 assays
- Total candidates: 20 compounds

## Strategy
Based on literature and drug-like properties:
- Compounds with clogP values between 1-3 are often preferred for BBB penetration
- Molecular weight under ~400-500 Da typically favorable
- TPSA values below ~90 are generally better for BBB penetration
- Will prioritize compounds that meet these criteria first

## Initial Analysis of Candidates
Looking at the data, promising candidates based on physicochemical properties:
- z001999: MW=299.4, clogP=1.5, TPSA=41.9
- z002010: MW=339.5, clogP=4.28, TPSA=29.5
- z001510: MW=180.2, clogP=-1.04, TPSA=72.7 (low clogP, might affect BBB)
- z002040: MW=217.3, clogP=1.77, TPSA=46.2
- z002746: MW=231.3, clogP=1.55, TPSA=30.2
- z003001: MW=237.7, clogP=2.9, TPSA=29.1

## Testing Results
- z001999: logS = -1.522, bbb = 1 → Does NOT qualify (logS > -2)
- z002040: logS = -2.3374, bbb = 1 → QUALIFIES ✓
- z002746: logS = -0.6286, bbb = 1 → Does NOT qualify (logS > -2)
- z003001: logS = -0.137, bbb = 1 → Does NOT qualify (logS > -2)
- z002010: logS = -5.0097, bbb = 1 → Does NOT qualify (logS < -3)
- z001510: logS = -2.7372, bbb = 1 → QUALIFIES ✓
- z002003: logS = -3.6, bbb = 1 → Does NOT qualify (logS < -3)
- z002011: logS = -4.13, bbb = 1 → Does NOT qualify (logS < -3)

## Final Results
Out of 8 assays used, I found 2 compounds that meet both criteria:
1. z002040: logS = -2.3374, bbb = 1
2. z001510: logS = -2.7372, bbb = 1

These are the only compounds that satisfy:
- logS between -3 and -2 (inclusive)
- bbb = 1