# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 34 measure_tc calls
- Need to complete Steps A-D

## Step A - Series Peak
Find maximum Tc in homologous doping series (same element set, varying oxygen) from data/measurable.txt.

Materials in measurable.txt (30 total):
1-13: O6.xCa1Cu2Ba2Hg1 series (12 materials)
14-24: O4.xCu1Ba2Hg1 series (11 materials)
25: O6Cu1Ba2Hg1Tl1
26: O10Ca2Cu3Ba2Tl2
27: O6Cu1Cd0.250Ba2Tl1.750
28: O7Cu3Ba2La1
29: O7Cu3Y0.200Ba2La0.800

The seed material O6.080Ca1Cu2Ba2Hg1 suggests the homologous series is O6.xCa1Cu2Ba2Hg1 (materials 1-13 in measurable.txt).

## Measurements So Far (Step A - O6.xCa1Cu2Ba2Hg1 series)
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
5. O6.160Ca1Cu2Ba2Hg1: No sample (in unmeasurable.txt)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K (high) - Current maximum
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) - New maximum
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) - Ties maximum
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
10. O6.270Ca1Cu2Ba2Hg1: No sample (in unmeasurable.txt)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

Current T* (maximum): 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## O4.xCu1Ba2Hg1 series measurements:
13. O4.081Cu1Ba2Hg1: 98.0 K (high)
14. O4.162Cu1Ba2Hg1: 97.4 K (high)
15. O4.150Cu1Ba2Hg1: 97.0 K (high)
16. O4.220Cu1Ba2Hg1: 97.0 K (high)
17. O4.096Cu1Ba2Hg1: 95.6 K (high)
18. O4.097Cu1Ba2Hg1: 95.3 K (high)
19. O4.200Cu1Ba2Hg1: 94.2 K (high)
20. O4.101Cu1Ba2Hg1: 93.4 K (high)
21. O4.103Cu1Ba2Hg1: 92.5 K (high)
22. O4.085Cu1Ba2Hg1: 92.0 K (high)

## Other materials measurements:
23. O6Cu1Ba2Hg1Tl1: 0.0 K (low) - Note: contains Hg and has 5 elements
24. O10Ca2Cu3Ba2Tl2: 118.97 K (high) - Contains Tl, not Hg
25. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low) - Contains Cd and Tl
26. O7Cu3Ba2La1: 89.6 K (low) - Contains La, no Hg
27. O7Cu3Y0.200Ba2La0.800: 81.0 K (low) - Contains Y and La

T* remains 127.0 K (no higher Tc found)

## Step B - Filter Family B
Find materials with Hg and exactly 4 elements, Tc >= threshold1 (T* - 32).

Current threshold1 = 127.0 - 32 =作用于= 95.0 K

## Step C - Extrapolate
Need to induce a rule from measurable materials to predict high/low for unmeasurable materials in data/unmeasurable.txt (11 materials).

## Step D - Final Pick
Among unmeasurable materials predicted high in step C, pick one with fewest elements (break ties by higher expected Tc).

## Induced Rule (Step C)
Based on analysis of all measurable materials, the rule for high Tc (>=90 K) is:
**A material has high Tc if it contains Hg but not Tl, OR if it contains Tl and Ca but not Hg.**

This rule correctly predicts all measurable materials:
- All O6.xCa1Cu2Ba2Hg1 (Hg, no Tl) → high ✓
- All O4.xCu1Ba2Hg1 (Hg, no Tl) → high ✓
- O6Cu1Ba2Hg1Tl1 (Hg + Tl) → low ✓
- O10Ca2Cu3Ba2Tl2 (Tl + Ca, no Hg) → high ✓
- O6Cu1Cd0.250Ba2Tl1.750 (Tl, no Ca, no Hg) → low ✓
- O7Cu3Ba2La1 (no Hg, no Tl) → low ✓
- O7Cu3Y0.200Ba2La0.800 (no Hg, no Tl) → low ✓

## Step B - Q1 Determination
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials with Hg and exactly 4 elements (O4.xCu1Ba2Hg1 series):
Measurable materials with Tc >= 95.0 K:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)

Unmeasurable materials with Hg and 4 elements (predicted Tc >= 95.0 K):
7. O4.180Cu1Ba2Hg1
8. O4.047Cu1Ba2Hg1
9. O4.066Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1

## Step C - Predictions for Unmeasurable Materials
Applying the induced rule:
1. O6.160Ca1Cu2Ba2Hg1: Hg, no Tl → high
2. O6.270Ca1Cu2Ba2Hg1: Hg, no Tl → high
3. O4.180Cu1Ba2Hg1: Hg, no Tl → high
4. O4.047Cu1Ba2Hg1: Hg, no Tl → high
5. O4.066Cu1Ba2Hg1: Hg, no Tl → high
6. O4.160Cu1Ba2Hg1: Hg, no Tl → high
7. O8Ca0.850Cu2.200Ba2Tl1.800: Tl + Ca, no Hg → high
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl + Ca, no Hg → high
9. O8Cu3Y1Ba1.900La0.100: no Hg, no Tl → low
10. O8Cu4Y1Ba1.870La0.130: no Hg, no Tl → low

## Step D - Final Pick
Among unmeasurable materials predicted high, those with fewest elements:
- O4.180Cu1Ba2Hg1 (4 elements)
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)

Break tie by higher expected Tc:
- O4.160Cu1Ba2Hg1 has x=0.160, closest to O4.162 (Tc=97.4 K)
- Expected Tc ~97.4 K (highest among the 4)

Final pick: O4.160Cu1Ba2Hg1

## Measurement Strategy
Need to measure all measurable materials to find T* and build rule for predictions. Budget: 34 calls, but only 30 measurable materials + need some for rule building.
Budget used: 25/34