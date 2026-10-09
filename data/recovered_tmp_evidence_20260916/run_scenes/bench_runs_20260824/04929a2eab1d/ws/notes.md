# Task Notes

## Objective
Find compounds that satisfy both criteria:
1. measured logS between -3 and -2 (inclusive)
2. measured bbb = 1

## Strategy
- Total budget: 8 assays
- Need to select compounds that are most likely to meet our criteria based on their properties
- Compounds with clogp values around 2-4 might be good candidates for BBB penetration
- Compounds with moderate polarity (TPSA values not too high) might have better chances

## Initial Analysis of Candidates
Looking at the clogp values which might indicate BBB penetration potential:
- z000364: clogp=2.87
- z001999: clogp=1.5
- z002003: clogp=4.28
- z002011: clogp=4.32
- z002015: clogp=4.14
- z002170: clogp=3.0
- z002683: clogp=2.55
- z002764: clogp=3.56
- z002891: clogp=4.72
- z003001: clogp=2.9

## Testing Plan
I'll prioritize compounds with clogp values in the range that suggests potential BBB penetration (2-5 range) and then check their solubility.

## Test Results
- z000364: logS=-0.9482, bbb=0 - Does not qualify (logS too high, bbb=0)
- z001999: logS=-1.522, bbb=1 - Does not qualify (logS too high, outside -3 to -2 range)
- z002003: logS=-3.6, bbb=1 - Does not qualify (logS too low, outside -3 to -2 range)
- z002170: logS=-2.92, bbb=1 - Qualifies! (logS in range [-3,-2], bbb=1)
- z002683: logS=-4.17, bbb=0 - Does not qualify (logS too low, bbb=0)
- z002764: logS=-2.9008, bbb=0 - Does not qualify (logS in range but bbb=0)
- z003001: logS=-0.137, bbb=1 - Does not qualify (logS too high, outside -3 to -2 range)
- z002011: logS=-4.13, bbb=1 - Does not qualify (logS too low, outside -3 to -2 range)