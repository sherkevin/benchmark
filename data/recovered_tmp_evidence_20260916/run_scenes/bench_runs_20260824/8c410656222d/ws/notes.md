# Task Notes

## Objective
Find compounds that satisfy both conditions:
1. measured logS between -3 and -2 (inclusive)
2. measured bbb = 1

Budget: 8 assays total

## Candidates Analysis
There are 20 compounds in the dataset. I need to select the most promising ones based on their properties to test with the limited budget.

Initial review of properties to identify potentially promising candidates:

- logS is related to solubility, typically estimated from molecular properties
- clogp (calculated logP) affects solubility and BBB penetration
- TPSA (topological polar surface area) also affects solubility and BBB penetration
- Generally, compounds with moderate lipophilicity (clogp around 2-5) and reasonable TPSA might be good candidates

## Testing Results So Far
- z001999: logS=-1.522 (too high), bbb=1
- z002003: logS=-3.6 (too low), bbb=1
- z002015: logS=-3.82 (too low), bbb=1
- z002170: logS=-2.92 (IN RANGE [-3,-2]), bbb=1 ✓ QUALIFIES!
- z002010: logS=-5.0097 (too low), bbb=1
- z002011: logS=-4.13 (too low), bbb=1
- z002040: logS=-2.3374 (IN RANGE [-3,-2]), bbb=1 ✓ QUALIFIES!
- z002052: logS=-2.7894 (IN RANGE [-3,-2]), bbb=1 ✓ QUALIFIES!

## Summary
I found 3 compounds that qualify:
1. z002170: logS=-2.92, bbb=1
2. z002040: logS=-2.3374, bbb=1
3. z002052: logS=-2.7894, bbb=1

All three have logS values in the required range [-3, -2] and bbb=1.