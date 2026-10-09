# Notes: Superconductor Tc Study

## Step A - Series Peak

Goal: Measure all materials in `data/measurable.txt` to find maximum Tc (T*).

### Measurement Plan:
1. First measure all materials in the homologous doping series (same element set O-Ca-Cu-Ba-Hg with varying oxygen)
2. Then measure other materials in the list
3. Track Tc values to find maximum

### Measurements:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: NOT MEASURABLE (no measurement exists) - appears in both lists
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: NOT MEASURABLE (no measurement exists) - appears in both lists
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

13. O4.081Cu1Ba2Hg1: Tc = 98.0 K
14. O4.162Cu1Ba2Hg1: Tc = 97.4 K
15. O4.150Cu1Ba2Hg1: Tc = 97.0 K
16. O4.220Cu1Ba2Hg1: Tc = 97.0 K
17. O4.096Cu1Ba2Hg1: Tc = 95.6 K
18. O4.097Cu1Ba2Hg1: Tc = 95.3 K
19. O4.180Cu1Ba2Hg1: NOT MEASURABLE (no measurement exists)
20. O4.047Cu1Ba2Hg1: NOT MEASURABLE (no measurement exists)
21. O4.200Cu1Ba2Hg1: Tc = 94.2 K
22. O4.101Cu1Ba2Hg1: Tc = 93.4 K
23. O4.103Cu1Ba2Hg1: Tc = 92.5 K
24. O4.085Cu1Ba2Hg1: Tc = 92.0 K

25. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
26. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K
27. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
28. O7Cu3Ba2La1: Tc = 89.6 K
29. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K

**Maximum Tc: 127.0 K** (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

**T* = 127.0 K**
**threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

## Step B - Filter Family B

Materials that contain Hg and have exactly 4 elements (format O4.xxxCu1Ba2Hg1):

Qualifying materials (Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓

Non-qualifying (Tc < 95.0 K):
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

Unmeasurable materials (no Tc measurement, cannot determine):
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

**Q1 = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"]**

## Step C - Induce Rule and Predict

### Current Analysis:
**High Tc (≥ 90 K):**
- All O6.xxxCa1Cu2Ba2Hg1 series (92.0-127.0 K)
- All O4.xxxCu1Ba2Hg1 series (92.0-98.0 K)
- O10Ca2Cu3Ba2Tl2 (118.97 K)

**Low Tc (< 90 K):**
- O6Cu1Ba2Hg1Tl1 (0.0 K) - contains Hg AND Tl
- O6Cu1Cd0.250Ba2Tl1.750 (88.0 K) - contains Cd AND Tl
- O7Cu3Ba2La1 (89.6 K) - contains La
- O7Cu3Y0.200Ba2La0.800 (81.0 K) - contains Y AND La

### Patterns:
1. Materials with Hg alone (O6.xxx, O4.xxx series) are consistently high Tc.
2. Materials with Tl can be high (O10Ca2Cu3Ba2Tl2) or low (O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750).
3. Materials with La tend to be low or borderline low.
4. The extremely low Tc (0.0 K) occurs for O6Cu1Ba2Hg1Tl1 which contains both Hg AND Tl.

### Hypothesis:
The interaction rule might involve combinations of specific elements:
- Hg + Tl → very low Tc (0.0 K)
- Cd + Tl → low Tc (88.0 K)
- La presence → low/borderline Tc
- Y + La → low Tc

But O10Ca2Cu3Ba2Tl2 has Tl without Hg/Cd/La and is high.

### Induced Rule:
**Materials have low Tc (< 90 K) if they contain:**
1. **Both Hg AND Tl** (extremely low, e.g., 0.0 K)
2. **Both Cd AND Tl** (low, e.g., 88.0 K)
3. **La** (with or without Y) (borderline to low, e.g., 81.0-89.6 K)

**All other materials (without these combinations) have high Tc (≥ 90 K).**

This rule correctly classifies all measurable materials:
- O6Cu1Ba2Hg1Tl1: Hg AND Tl → LOW ✓ (0.0 K)
- O6Cu1Cd0.250Ba2Tl1.750: Cd AND Tl → LOW ✓ (88.0 K)
- O7Cu3Ba2La1: contains La → LOW ✓ (89.6 K)
- O7Cu3Y0.200Ba2La0.800: contains La → LOW ✓ (81.0 K)
- All O6.xxxCa1Cu2Ba2Hg1: Hg but no Tl → HIGH ✓ (92.0-127.0 K)
- All O4.xxxCu1Ba2Hg1: Hg but no Tl → HIGH ✓ (92.0-98.0 K)
- O10Ca2Cu3Ba2Tl2: Tl but no Hg/Cd/La → HIGH ✓ (118.97 K)

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Hg but no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg but no Tl → HIGH
3. O4.180Cu1Ba2Hg1: Hg but no Tl → HIGH
4. O4.047Cu1Ba2Hg1: Hg but no Tl → HIGH
5. O4.066Cu1Ba2Hg1: Hg but no Tl → HIGH
6. O4.160Cu1Ba2Hg1: Hg but no Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: Tl but no Hg/Cd/La → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl but no Hg/Cd/La → HIGH
9. O8Cu3Y1Ba1.900La0.100: contains La → LOW
10. O8Cu4Y1Ba1.870La0.130: contains La → LOW

## Step D - Final Pick

Among unmeasurable materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements)

Materials with fewest elements (4): O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1.

To break tie by higher expected Tc: Compare to measured O4.xxx series.
- O4.081 (98.0 K) highest in series
- O4.066 is closest to O4.081 (oxygen 4.066 vs 4.081)
- O4.047 is also close but slightly further
- O4.160 and O4.180 are further in oxygen content

**Choose O4.066Cu1Ba2Hg1 as final pick** (4 elements, expected high Tc similar to O4.081).

## Task Completion Summary

All steps completed:
1. **Step A**: Found maximum Tc = 127.0 K (O6.220Ca1Cu2Ba2Hg1)
2. **Step B**: Calculated threshold = 95.0 K, identified 6 qualifying materials (Q1)
3. **Step C**: Induced rule for high/low Tc classification, predicted all 10 unmeasurable materials
4. **Step D**: Selected O4.066Cu1Ba2Hg1 as final pick (fewest elements among predicted high)

Answer.json created with all required fields.

Budget: 45 total measurements, 27 used, 18 remaining.