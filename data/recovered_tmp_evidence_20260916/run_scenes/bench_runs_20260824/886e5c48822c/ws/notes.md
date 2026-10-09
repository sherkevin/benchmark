# Compound Triage Notes

## Initial Analysis
- Total compounds: 20
- Budget for assays: 8
- Target profile:
  - logS between -3 and -2 (inclusive)
  - bbb = 1

## Strategy
Given the limited budget of 8 assays, I will prioritize compounds that seem most likely to meet our criteria based on their properties:
- Compounds with clogp values that might suggest reasonable solubility
- Compounds with molecular weights and other descriptors that might indicate BBB penetration

## Testing Log

Test 1: z001510
- Result: logS = -2.7372, bbb = 1
- Qualifies: YES (meets both criteria)

Test 2: z001999
- Result: logS = -1.522, bbb = 1
- Qualifies: NO (logS > -2)

Test 3: z002003
- Result: logS = -3.6, bbb = 1
- Qualifies: NO (logS < -3)

Test 4: z002010
- Result: logS = -5.0097, bbb = 1
- Qualifies: NO (logS < -3)

Test 5: z002011
- Result: logS = -4.13, bbb = 1
- Qualifies: NO (logS < -3)

Test 6: z002015
- Result: logS = -3.82, bbb = 1
- Qualifies: NO (logS < -3)

Test 7: z002040
- Result: logS = -2.3374, bbb = 1
- Qualifies: YES (meets both criteria)

Test 8: z002052
- Result: logS = -2.7894, bbb = 1
- Qualifies: YES (meets both criteria)

## Summary
- Total assays performed: 8 (used full budget)
- Qualifying compounds found: 3
  1. z001510 (logS = -2.7372, bbb = 1)
  2. z002040 (logS = -2.3374, bbb = 1)
  3. z002052 (logS = -2.7894, bbb = 1)