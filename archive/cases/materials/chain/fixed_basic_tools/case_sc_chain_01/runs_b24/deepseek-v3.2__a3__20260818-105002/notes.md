# Superconductor Tc Study Notes

## Step A: Series Peak Analysis

### Measurements Plan
1. Measure all materials in `data/measurable.txt` (26 materials + 1 seed = 27 total)
2. Identify maximum Tc (T*)
3. Record composition with maximum Tc

### All Measurements Completed (Budget: 24/24 used)
- O6.080Ca1Cu2Ba2Hg1 (seed): Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
- O4.081Cu1Ba2Hg1: Tc = 98.0 K
- O4.162Cu1Ba2Hg1: Tc = 97.4 K
- O4.150Cu1Ba2Hg1: Tc = 97.0 K
- O4.220Cu1Ba2Hg1: Tc = 97.0 K
- O4.096Cu1Ba2Hg1: Tc = 95.6 K
- O4.097Cu1Ba2Hg1: Tc = 95.3 K
- O4.047Cu1Ba2Hg1: Tc = 95.0 K
- O4.200Cu1Ba2Hg1: Tc = 94.2 K
- O4.101Cu1Ba2Hg1: Tc = 93.4 K
- O4.103Cu1Ba2Hg1: Tc = 92.5 K
- O4.085Cu1Ba2Hg1: Tc = 92.0 K
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K

**Note**: Materials O7Cu3Ba2La1 and O7Cu3Y0.200Ba2La0.800 were not measured due to budget constraints.

### Step A Results
Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition = O6.210Ca1Cu2Ba2Hg1 (first occurrence)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B: Filter Family B (contains Hg and has exactly 4 elements)

### Materials that contain Hg and have exactly 4 elements (from measurable.txt):
1. O4.081Cu1Ba2Hg1 (Tc = 98.0 K) ✓
2. O4.162Cu1Ba2Hg1 (Tc = 97.4 K) ✓
3. O4.150Cu1Ba2Hg1 (Tc = 97.0 K) ✓
4. O4.220Cu1Ba2Hg1 (Tc = 97.0 K) ✓
5. O4.096Cu1Ba2Hg1 (Tc = 95.6 K) ✓
6. O4.097Cu1Ba2Hg1 (Tc = 95.3 K) ✓
7. O4.047Cu1Ba2Hg1 (Tc = 95.0 K) ✓ (equal to threshold)
8. O4.200Cu1Ba2Hg1 (Tc =115.0) ✗ (Wait, I made an error - this actually has Tc = 94.2 K)
9. O4.101Cu1Ba2Hg1 (Tc = 93.4 K) ✗
10. O4.103Cu1Ba2Hg1 (Tc = 92.5 K) ✗
11. O4.085Cu1Ba2Hg1 (Tc = 92.0 K) ✗

**Correction**: Let me check O4.200Cu1Ba2Hg1 measurement again - it was 94.2 K which is < 95.0 K.

### Step B Results (Q1):
Qualifying materials (Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.047Cu1Ba2Hg1

## Step C: Induce Rule for High/Low Tc Prediction

### Analysis of Measured Materials:
Let me categorize all measured materials as High (Tc ≥ 90 K) or Low (Tc < 90 K):

**High Tc (≥90 K):**
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (5 elements: O, Ca, Cu, Ba, Hg)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (5 elements)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (5 elements)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (5 elements)
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K (5 elements)
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (5 elements)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (5 elements)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K (5 elements)
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K (5 elements)
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K (5 elements)
11. O4.081Cu1Ba2Hg1: 98.0 K (4 elements: O, Cu, Ba, Hg)
12. O4.162Cu1Ba2Hg1: 97.4 K (4 elements)
13. O4.150Cu1Ba2Hg1: 97.0 K (4 elements)
14. O4.220Cu1Ba2Hg1: 97.0 K (4 elements)
15. O4.096Cu1Ba2Hg1: 95.6 K (4 elements)
16. O4.097Cu1Ba2Hg1: 95.3 K (4 elements)
17. O4.047Cu1Ba2Hg1: 95.0 K (4 elements)
18. O4.200Cu1Ba2Hg1: 94.2 K (4 elements)
19. O4.101Cu1Ba2Hg1: 93.4 K (4 elements)
20. O4.103Cu1Ba2Hg1: 92.5 K (4 elements)
21. O4.085Cu1Ba2Hg1: 92.0 K (4 elements)
22. O10Ca2Cu3Ba2Tl2: 118.97 K (5 elements: O, Ca, Cu, Ba, Tl)
23. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (5 elements: O, Cu, Cd, Ba, Tl) - LOW

**Low Tc (<90 K):**
1. O6Cu1Ba2Hg1Tl1: 0.0 K (5 elements: O, Cu, Ba, Hg, Tl)
2. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (5 elements)

**Not measured due to budget:**
1. O7Cu3Ba2La1 (elements: O, Cu, Ba, La) - 4 elements
2. O7Cu3Y0.200Ba2La0.800 (elements: O, Cu, Y, Ba, La) - 5 elements

### Observations:
1. All O6Ca1Cu2Ba2Hg1 series materials are High Tc (92-127 K)
2. All O4Cu1Ba2Hg1 series materials are High Tc (92-98 K)
3. O10Ca2Cu3Ba2Tl2 is High Tc (118.97 K)
4. O6Cu1Ba2Hg1Tl1 is Low Tc (0 K) - contains Hg and Tl
5. O6Cu1Cd0.250Ba2Tl1.750 is Low Tc (88 K) - contains Cd and Tl, no Hg

### Induced Rule Analysis:

**Hypothesis Tested**: "Materials containing Tl but not Ca are Low Tc (<90 K)"

**Evidence from measured materials**:
1. O6Cu1Ba2Hg1Tl1: Contains Tl, no Ca → Tc = 0 K (Low) ✓
2. O6Cu1Cd0.250Ba2Tl1.750: Contains Tl, no Ca → Tc = 88 K (Low) ✓
3. O10Ca2Cu3Ba2Tl2: Contains Tl AND Ca → Tc = 118.97 K (High) ✓
4. All O6Ca1Cu2Ba2Hg1 series: No Tl → All High Tc ✓
5. All O4Cu1Ba2Hg1 series: No Tl → All High Tc ✓

**Proposed Rule**: 
"Materials are High Tc (≥90 K) if they either: 
1. Do NOT contain Tl, OR 
2. Contain BOTH Tl AND Ca.
Materials containing Tl but NOT Ca are Low Tc (<90 K)."

This rule correctly classifies all 24 measured materials.

### Applying Rule to Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1: No Tl → **High**
2. O6.270Ca1Cu2Ba2Hg1: No Tl → **High**
3. O4.180Cu1Ba2Hg1: No Tl → **High**
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl AND Ca → **High**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl AND Ca → **High**
6. O8Cu3Y1Ba1.900La0.100: No Tl → **High**
7. O8Cu4Y1Ba1.870La0.130: No Tl → **High**

**Prediction**: All unmeasurable materials are High Tc according to this rule.

### Step D: Final Pick
Among unmeasurable materials predicted High in Step C, return the one with the FEWEST elements (break ties by higher expected Tc).

Element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)
6. O8Cu3Y1Ba1.900La0.100: 5 elements (O, Cu, Y, Ba, La)
7. O8Cu4Y1Ba1.870La0.130: 5 elements (O, Cu, Y, Ba, La)

Material with fewest elements: O4.180Cu1Ba2Hg1 (4 elements)

**Final Answer**: O4.180Cu1Ba2Hg1