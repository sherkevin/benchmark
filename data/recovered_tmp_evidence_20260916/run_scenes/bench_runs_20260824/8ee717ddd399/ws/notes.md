# Notes on Compound Triage

## Task
Find compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays

## Strategy
Select compounds with BBB-friendly properties:
- TPSA < 90
- clogP in moderate range (1-4)
- Low HBD count

## Assays Run

| Mol ID | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002010 | -5.0097 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002040 | -2.3374 | 1 | YES |
| z002052 | -2.7894 | 1 | YES |
| z002170 | -2.92 | 1 | YES |

Budget used: 8/8

## Candidates to test (prioritized)
1. z001999: TPSA=41.9, clogP=1.5, HBD=1
2. z002003: TPSA=43.1, clogP=3.58, HBD=0
3. z002010: TPSA=29.5, clogP=4.28, HBD=0
4. z002011: TPSA=30.2, clogP=4.32, HBD=0
5. z002015: TPSA=23.6, clogP=4.14, HBD=0
6. z002040: TPSA=46.2, clogP=1.77, HBD=1
7. z002052: TPSA=65.1, clogP=1.99, HBD=0
8. z002170: TPSA=34.9, clogP=3.0, HBD=0
9. z002746: TPSA=30.2, clogP=1.55, HBD=0
10. z003001: TPSA=29.1, clogP=2.9, HBD=1
