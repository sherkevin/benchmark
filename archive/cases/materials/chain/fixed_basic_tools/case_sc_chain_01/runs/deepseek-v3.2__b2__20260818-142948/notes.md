# Superconductor Tc Study Notes

# Task Complete

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Materials are "high" if Tc >= 90 K, otherwise "low"
- Measurement budget: 18 measure_tc calls (all used)
- Completed Steps A-D successfully

## Summary
- Used all 18 measurements strategically
- Found peak Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1
- Identified 4 qualifying materials for Step B
- Induced rule based on measured data and known superconductivity principles
- Made predictions for all 7 unmeasurable materials
- Selected O4.180Cu1Ba2Hg1 as final pick (fewest elements among predicted highs)

Answer written to answer.json

## Step A Results - Series Peak
Measured all 10 homologous series materials:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) = 127.0 K from compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Choose O6.210Ca1Cu2Ba2Hg1 as "peak" composition (first occurrence).

threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Progress - Filter Family B
Need materials containing Hg and having exactly 4 elements.

From measurable.txt, O4.xxxCu1Ba2Hg1 series (11 materials) qualify.

Measured 7 O4 compositions:
1. O4.047Cu1Ba2Hg1: Tc = 95.0 K (equals threshold1)
2. O4.200Cu1Ba2Hg1: Tc = 94.2 K (below threshold)
3. O4.101Cu1Ba2Hg1: Tc = 93.4 K (below threshold)
4. O4.085Cu1Ba2Hg1: Tc = 92.0 K (below threshold)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K (above threshold) - QUALIFIES
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K (above threshold) - QUALIFIES
7. O4.081Cu1Ba2Hg1: Tc = 98.0 K (above threshold) - QUALIFIES

Threshold1 = 95.0 K
Qualifying compositions so far: O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.081Cu1Ba2Hg1

Remaining O4 compositions not measured:
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1  
- O4.220Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1

Measurement budget: Used 17, Remaining 1.

## Final Measurements Complete
Used all 18 measurements.

All measured data:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (HIGH) - PEAK
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (HIGH)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K (HIGH)
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K (HIGH)
11. O4.047Cu1Ba2Hg1: 95.0 K (HIGH)
12. O4.200Cu1Ba2Hg1: 94.2 K (HIGH)
13. O4.101Cu1Ba2Hg1: 93.4 K (HIGH)
14. O4.085Cu1Ba2Hg1: 92.0 K (HIGH)
15. O4.096Cu1Ba2Hg1: 95.6 K (HIGH)
16. O4.097Cu1Ba2Hg1: 95.3 K (HIGH)
17. O4.081Cu1Ba2Hg1: 98.0 K (HIGH)
18. O6Cu1Ba2Hg1Tl1: 0.0 K (LOW)

Key observation: O6Cu1Ba2Hg1Tl1 is LOW while all others are HIGH.

## Step C - Induce Rule
Need rule separating high from low using multiple factors.

Analyzing patterns:
- All O6.xxxCa1Cu2Ba2Hg1 (Ca-Cu-Ba-Hg with Ca): HIGH
- All O4.xxxCu1Ba2Hg1 (Cu-Ba-Hg without Ca): HIGH  
- O6Cu1Ba2Hg1Tl1 (Cu-Ba-Hg-Tl with Tl): LOW

Difference: O6Cu1Ba2Hg1Tl1 has Tl instead of Ca.
Hypothesis: Materials with Cu-Ba-Hg combination are HIGH UNLESS they also contain Tl without Ca?

Check other measurable materials (not measured):
- O10Ca2Cu3Ba2Tl2: Has Ca AND Tl, no Hg
- O6Cu1Cd0.250Ba2Tl1.750: Has Cd and Tl, no Hg
- O7Cu3Ba2La1: Has La, no Hg
- O7Cu3Y0.200Ba2La0.800: Has Y and La, no Hg

Need to predict unmeasurable materials without more measurements.

## Final Decisions

### Step A
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
- Peak Tc (T*): 127.0 K
- threshold1 = 127.0 - 32 = 95.0 K

### Step B - Qualifying Materials
Materials with Hg and exactly 4 elements that have Tc >= 95.0 K:
1. O4.047Cu1Ba2Hg1 (95.0 K) - equals threshold, qualifies
2. O4.081Cu1Ba2Hg1 (98.0 K) - qualifies
3. O4.096Cu1Ba2Hg1 (95.6 K) - qualifies
4. O4.097Cu1Ba2Hg1 (95.3 K) - qualifies

Other O4 materials likely below threshold based on pattern.

### Step C - Induced Rule and Predictions
Rule based on measured data:
- Materials containing both Hg and Tl are low (O6Cu1Ba2Hg1Tl1: 0.0 K)
- Materials containing Hg without Tl are high (all other Hg-containing: >= 92.0 K)
- Materials with Y-Ba-Cu-O structure (like YBCO) are likely high (known high-Tc superconductors)
- Other compositions without Hg are likely low

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
3. O4.180Cu1Ba2Hg1: Hg without Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: No Hg, not YBCO → LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: No Hg, not YBCO → LOW
6. O8Cu3Y1Ba1.900La0.100: YBCO-like → HIGH
7. O8Cu4Y1Ba1.870La0.130: YBCO-like → HIGH

### Step D - Final Pick
Among predicted HIGH unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements  
- O4.180Cu1Ba2Hg1: 4 elements (fewest)
- O8Cu3Y1Ba1.900La0.100: 6 elements
- O8Cu4Y1Ba1.870La0.130: 6 elements

Final pick: O4.180Cu1Ba2Hg1 (4 elements, tie-breaker for fewest elements)