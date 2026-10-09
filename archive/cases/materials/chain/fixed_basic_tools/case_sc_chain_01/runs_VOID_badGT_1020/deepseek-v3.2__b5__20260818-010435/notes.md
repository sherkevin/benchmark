# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 34 `measure_tc` calls
- High Tc defined as >= 90 K

## Step A - Series Peak
Need to measure all compositions in `data/measurable.txt` (30 total) to find maximum Tc (T*).
Budget: 30 measurements for Step A leaves 4 for Step B.

## Step B - Filter Family B
Find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= threshold1 (T* - 32)

## Step C - Extrapolate
Need to induce a rule from measurable materials to predict high/low for unmeasurable materials.

## Step D - Final Pick
Among predicted high Tc unmeasurable materials, pick one with fewest elements.

## Plan
1. Measure all 30 measurable compositions to find T*
2. Calculate threshold1 = T* - 32
3. Identify materials in measurable set that contain Hg and have exactly 4 elements
4. Check which of those have Tc >= threshold1 (Q1)
5. Analyze data to find rule for predicting high/low Tc
6. Apply rule to unmeasurable materials
7. Select final material

## Measurements
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
13. O4.081Cu1Ba2Hg1: Tc = 98.0 K
14. O4.162Cu1Ba2Hg1: Tc = 97.4 K
15. O4.150Cu1Ba2Hg1: Tc = 97.0 K
16. O4.220Cu1Ba2Hg1: Tc = 97.0 K
17. O4.096Cu1Ba2Hg1: Tc = 95.6 K
18. O4.097Cu1Ba2Hg1: Tc = 95.3 K
19. O4.180Cu1Ba2Hg1: No sample (unmeasurable)
20. O4.047Cu1Ba2Hg1: No sample (unmeasurable)
21. O4.200Cu1Ba2Hg1: Tc = 94.2 K
22. O4.101Cu1Ba2Hg1: Tc = 93.4 K
23. O4.103Cu1Ba2Hg1: Tc = 92.5 K
24. O4.085Cu1Ba2Hg1: Tc = 92.0 K
25. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
26. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K
27. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
28. O7Cu3Ba2La1: Tc = 89.6 K
29. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K
Budget used: 25/34 (4 attempts on unmeasurable samples didn't count toward budget)
Maximum Tc (T*): 127.0 K

## Step A Complete
Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have Tc = 127.0 K)
T* = 127.0 K

## Step B Analysis
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials that contain Hg and have exactly 4 elements (O4.*Cu1Ba2Hg1 series):
1. O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
2. O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
4. O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
5. O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 K ✓
6. O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 K ✓
7. O4.200Cu1Ba2Hg1: 94.2 K < 95.0 K ✗
8. O4.101Cu1Ba2Hg1: 93.4 K < 95.0 K ✗
9. O4.103Cu1Ba2Hg1: 92.5 K < 95.0 K ✗
10. O4.085Cu1Ba2Hg1: 92.0 K < 95.0 K ✗

Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

## Step C - Extrapolate
Now need to induce a rule to predict high/low Tc for unmeasurable materials.

### Induced Rule
Based on analysis of measurable materials:

**HIGH Tc materials**: Do NOT contain any of these "bad" features:
1. Contains both Hg AND Tl
2. Contains both Cd AND Tl
3. Contains La (with or without Y)
4. Contains both Y AND La

**LOW Tc materials**: Contain at least one of the "bad" features above.

### Testing rule on measurable materials:
High Tc (correct predictions):
- O10Ca2Cu3Ba2Tl2: Has Tl alone → HIGH ✓
- All O6.*Ca1Cu2Ba2Hg1: Has Hg alone → HIGH ✓
- All O4.*Cu1Ba2Hg1: Has Hg alone → HIGH ✓

Low Tc (correct predictions):
- O6Cu1Ba2Hg1Tl1: Has Hg AND Tl → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: Has Cd AND Tl → LOW ✓
- O7Cu3Ba2La1: Has La → LOW ✓
- O7Cu3Y0.200Ba2La0.800: Has Y AND La → LOW ✓

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
3. O4.180Cu1Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
4. O4.047Cu1Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
5. O4.066Cu1Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
6. O4.160Cu1Ba2Hg1: Contains Hg, NOT Tl, NOT Cd, NOT La, NOT Y → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl, NOT Hg, NOT Cd, NOT La, NOT Y → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl, NOT Hg, NOT Cd, NOT La, NOT Y → HIGH
9. O8Cu3Y1Ba1.900La0.100: Contains Y AND La → LOW
10. O8Cu4Y1Ba1.870La0.130: Contains Y AND La → LOW

## Step D - Final Pick
Among predicted HIGH Tc unmeasurable materials, pick one with FEWEST elements.

Predicted HIGH Tc unmeasurable materials with element counts:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.180Cu1Ba2Hg1 - 4 elements
4. O4.047Cu1Ba2Hg1 - 4 elements
5. O4.066Cu1Ba2Hg1 - 4 elements
6. O4.160Cu1Ba2Hg1 - 4 elements
7. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 - 6 elements

Materials with fewest elements (4): O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1

Breaking ties by higher expected Tc based on O4 series trend:
- O4.047: Lowest oxygen, likely lower Tc
- O4.066: Slightly higher oxygen than 4.047
- O4.160: Between O4.150 (97.0 K) and O4.162 (97.4 K)
- O4.180: Between O4.162 (97.4 K) and O4.200 (94.2 K)

O4.160 likely has highest expected Tc (~97.2 K)

Final pick: O4.160Cu1Ba2Hg1

## Summary
- Peak composition: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold1: 95.0 K
- Q1: 6 qualifying materials
- Predictions: 8 HIGH, 2 LOW
- Final pick: O4.160Cu1Ba2Hg1
- Rule: See answer.json for full rule description

Task completed! Budget used: 25/34 measurements.