# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, low Tc < 90 K
- Measurement budget: 34 measure_tc calls
- Need to complete 4 steps

## Step A - Series Peak
Need to measure homologous doping series (same element set, varying oxygen) in data/measurable.txt to find maximum Tc (T*).

Looking at measurable.txt:
- Lines 1-12: O6.xxxCa1Cu2Ba2Hg1 series (12 compositions)
- Lines 13-24: O4.xxxCu1Ba2Hg1 series (12 compositions)
- Lines 25-29: Various other compositions (5 compositions)

Total measurable compositions: 29

## Step B - Filter Family B
Threshold1 = T* - 32
Among materials that contain Hg and have exactly 4 elements, report all with Tc >= threshold1 (set Q1).

## Step C - Extrapolate
Unmeasurable materials (11 compositions) can't be measured directly.
Need to induce rule separating high/low Tc using measurable materials, then predict high/low for unmeasurable materials.

## Step D - Final Pick
Among unmeasurable materials predicted high in step C, return one with fewest elements (break ties by higher expected Tc).

## Initial Observations
- Seed material is O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- Some compositions appear in both files (e.g., O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1) - these are in the "unmeasurable" list meaning they haven't been synthesized yet
- Measurement budget of 34 calls means we can measure all 29 measurable compositions with 5 calls to spare

## Measurements Record

### O6.xxxCa1Cu2Ba2Hg1 Series (12 compositions)
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K ✓

Maximum Tc in O6.xxx series (measurable ones): 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

### O4.xxxCu1Ba2Hg1 Series (12 compositions)
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓
7. O4.180Cu1Ba2Hg1: No sample (unmeasurable)
8. O4.047Cu1Ba2Hg1: No sample (unmeasurable)
9. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✓
10. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✓
11. O4.103Cu1Ba2Hg1: Tc = 92.5 K ✓
12. O4.085Cu1Ba2Hg1: Tc = 92.0 K ✓

Maximum Tc in O4.xxx series (measurable ones): 98.0 K (O4.081Cu1Ba2Hg1)

### Other Compositions (5 compositions)
1. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K ✓
2. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K ✓
3. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K ✓
4. O7Cu3Ba2La1: Tc = 89.6 K ✓
5. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K ✓

## Step A Analysis
From all measurable compositions, maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Analysis
Q1 = Materials with exactly 4 elements, containing Hg, and Tc >= 95.0 K:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)

## Step C Analysis - Rule Induction

### Measurable Materials Classification:
High Tc (>= 90 K):
1. O6.080Ca1Cu2Ba2Hg1 (92.0) - elements: O,Ca,Cu,Ba,Hg
2. O6.100Ca1Cu2Ba2Hg1 (92.5) - elements: O,Ca,Cu,Ba,Hg
3. O6.130Ca1Cu2Ba2Hg1 (102.0) - elements: O,Ca,Cu,Ba,Hg
4. O6.140Ca1Cu2Ba2Hg1 (105.0) - elements: O,Ca,Cu,Ba,Hg
5. O6.190Ca1Cu2Ba2Hg1 (122.0) - elements: O,Ca,Cu,Ba,Hg
6. O6.210Ca1Cu2Ba2Hg1 (127.0) - elements: O,Ca,Cu,Ba,Hg
7. O6.220Ca1Cu2Ba2Hg1 (127.0) - elements: O,Ca,Cu,Ba,Hg
8. O6.240Ca1Cu2Ba2Hg1 (123.0) - elements: O,Ca,Cu,Ba,Hg
9. O6.300Ca1Cu2Ba2Hg1 (100.0) - elements: O,Ca,Cu,Ba,Hg
10. O6.310Ca1Cu2Ba2Hg1 (98.0) - elements: O,Ca,Cu,Ba,Hg
11. O4.081Cu1Ba2Hg1 (98.0) - elements: O,Cu,Ba,Hg
12. O4.162Cu1Ba2Hg1 (97.4) - elements: O,Cu,Ba,Hg
13. O4.150Cu1Ba2Hg1 (97.0) - elements: O,Cu,Ba,Hg
14. O4.220Cu1Ba2Hg1 (97.0) - elements: O,Cu,Ba,Hg
15. O4.096Cu1Ba2Hg1 (95.6) - elements: O,Cu,Ba,Hg
16. O4.097Cu1Ba2Hg1 (95.3) - elements: O,Cu,Ba,Hg
17. O4.200Cu1Ba2Hg1 (94.2) - elements: O,Cu,Ba,Hg
18. O4.101Cu1Ba2Hg1 (93.4) - elements: O,Cu,Ba,Hg
19. O4.103Cu1Ba2Hg1 (92.5) - elements: O,Cu,Ba,Hg
20. O4.085Cu1Ba2Hg1 (92.0) - elements: O,Cu,Ba,Hg
21. O10Ca2Cu3Ba2Tl2 (118.97) - elements: O,Ca,Cu,Ba,Tl

Low Tc (< 90 K):
1. O6Cu1Ba2Hg1Tl1 (0.0) - elements: O,Cu,Ba,Hg,Tl
2. O6Cu1Cd0.250Ba2Tl1.750 (88.0) - elements: O,Cu,Cd,Ba,Tl
3. O7Cu3Ba2La1 (89.6) - elements: O,Cu,Ba,La
4. O7Cu3Y0.200Ba2La0.800 (81.0) - elements: O,Cu,Y,Ba,La

### Observations:
1. All high Tc materials contain Ba
2. All high Tc materials contain Cu (but all materials contain Cu)
3. High Tc materials either:
   - Contain Hg (all O4.xxxCu1Ba2Hg1 and O6.xxxCa1Cu2Ba2Hg1)
   - OR contain Tl AND Ca (O10Ca2Cu3Ba2Tl2)
4. Low Tc materials with Hg+Tl (O6Cu1Ba2Hg1Tl1) are low
5. Low Tc materials with Tl but no Ca (O6Cu1Cd0.250Ba2Tl1.750) are low
6. Low Tc materials without Hg or Tl (O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800) are low

### Possible Rule:
Material is HIGH Tc if:
1. Contains Ba AND
2. Contains Cu AND
3. (Contains Hg) OR (Contains Tl AND Contains Ca)

Test this rule on measurable materials:
- All high Tc materials pass ✓
- O6Cu1Ba2Hg1Tl1: has Ba, Cu, Hg → should be high but is actually low ✗ (rule fails here!)
- O6Cu1Cd0.250Ba2Tl1.750: has Ba, Cu, Tl but no Ca → should be low ✓
- O7Cu3Ba2La1: has Ba, Cu, no Hg/Tl → should be low ✓
- O7Cu3Y0.200Ba2La0.800: has Ba, Cu, no Hg/Tl → should be low ✓

The rule fails for O6Cu1Ba2Hg1Tl1. Maybe need additional condition about element count or specific combinations.

## Final Rule Induced
**Material has HIGH Tc (>= 90 K) if it contains Barium (Ba) AND ((contains Mercury (Hg) AND NOT contains Thallium (Tl)) OR (contains Thallium (Tl) AND contains Calcium (Ca)))**

This rule correctly classifies all 25 measurable materials (21 high, 4 low).

## Step C Predictions for Unmeasurable Materials
1. O6.160Ca1Cu2Ba2Hg1: high (Ba, Hg, no Tl)
2. O6.270Ca1Cu2Ba2Hg1: high (Ba, Hg, no Tl)
3. O4.180Cu1Ba2Hg1: high (Ba, Hg, no Tl)
4. O4.047Cu1Ba2Hg1: high (Ba, Hg, no Tl)
5. O4.066Cu1Ba2Hg1: high (Ba, Hg, no Tl)
6. O4.160Cu1Ba2Hg1: high (Ba, Hg, no Tl)
7. O8Ca0.850Cu2.200Ba2Tl1.800: high (Ba, Tl, Ca)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: low (no Ba, has Sr instead)
9. O8Cu3Y1Ba1.900La0.100: low (Ba, no Hg/Tl)
10. O8Cu4Y1Ba1.870La0.130: low (Ba, no Hg/Tl)

## Step D Final Pick
Among high predictions, materials with fewest elements (4 elements):
- O4.180Cu1Ba2Hg1 (4 elements)
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)

Tie broken by higher expected Tc. Chose O4.047Cu1Ba2Hg1.

## Final Answer Summary
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold1: 95.0 K
- Q1: 6 O4.xxxCu1Ba2Hg1 compositions with Tc >= 95.0 K
- Predictions: 7 high, 3 low
- Final pick: O4.047Cu1Ba2Hg1
- Rule: As stated above