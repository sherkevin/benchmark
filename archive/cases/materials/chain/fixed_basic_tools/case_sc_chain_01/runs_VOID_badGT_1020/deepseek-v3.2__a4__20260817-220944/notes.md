# Superconductor Tc Study Notes

## Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 34 measure_tc calls

## Step A - Series Peak
Need to measure homologous doping series members in data/measurable.txt (same element set, varying oxygen).

From the file, the homologous series appears to be:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - 105.0 K
5. O6.160Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1 - 122.0 K
7. O6.210Ca1Cu2Ba2Hg1 - 127.0 K
8. O6.220Ca1Cu2Ba2Hg1 - 127.0 K
9. O6.240Ca1Cu2Ba2Hg1 - 123.0 K
10. O6.270Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1 - 100.0 K
12. O6.310Ca1Cu2Ba2Hg1 - 98.0 K

Maximum Tc (T*) among measurable compositions: 127.0 K (for O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Chosen peak composition: O6.210Ca1Cu2Ba2Hg1 (T* = 127.0 K)

## Step B - Filter Family B
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0 K

From measurable.txt, the O4.xxxCu1Ba2Hg1 series qualifies (contains Hg, elements: O, Cu, Ba, Hg = 4 elements).

Measurements:
1. O4.081Cu1Ba2Hg1 - 98.0 K ✓
2. O4.162Cu1Ba2Hg1 - 97.4 K ✓
3. O4.150Cu1Ba2Hg1 - 97.0 K ✓
4. O4.220Cu1Ba2Hg1 - 97.0 K ✓
5. O4.096Cu1Ba2Hg1 - 95.6 K ✓
6. O4.097Cu1Ba2Hg1 - 95.3 K ✓
7. O4.180Cu1Ba2Hg1 - Unmeasurable (no sample)
8. O4.047Cu1Ba2Hg1 - Unmeasurable (no sample)
9. O4.200Cu1Ba2Hg1 - 94.2 K ✗
10. O4.101Cu1Ba2Hg1 - 93.4 K ✗
11. O4.103Cu1Ba2Hg1 - 92.5 K ✗
12. O4.085Cu1Ba2Hg1 - 92.0 K ✗

Q1 (qualifying set): [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

## Step C - Rule Induction
Need to induce rule separating high (>= 90 K) from low (< 90 K) Tc based on measurable compositions.

All measurable compositions and Tc values:

High Tc (>= 90 K):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K
11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O4.150Cu1Ba2Hg1: 97.0 K
14. O4.220Cu1Ba2Hg1: 97.0 K
15. O4.096Cu1Ba2Hg1: 95.6 K
16. O4.097Cu1Ba2Hg1: 95.3 K
17. O4.200Cu1Ba2Hg1: 94.2 K
18. O4.101Cu1Ba2Hg1: 93.4 K
19. O4.103Cu1Ba2Hg1: 92.5 K
20. O4.085Cu1Ba2Hg1: 92.0 K
21. O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc (< 90 K):
1. O6Cu1Ba2Hg1Tl1: 0.0 K
2. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
3. O7Cu3Ba2La1: 89.6 K
4. O7Cu3Y0.200Ba2La0.800: 81.0 K

Observations:
- All compositions containing only Hg (no Tl) have high Tc
- Composition containing only Tl (O10Ca2Cu3Ba2Tl2) has high Tc
- Composition containing both Hg AND Tl (O6Cu1Ba2Hg1Tl1) has low Tc
- Compositions containing La tend to have low Tc (borderline for O7Cu3Ba2La1 at 89.6 K)
- Composition containing Cd has low Tc

Induced Rule: High Tc (>= 90 K) if and only if the composition contains either Hg or Tl (but not both) AND does not contain Cd, La, or Y.

Applied to unmeasurable compositions:
1. O6.160Ca1Cu2Ba2Hg1: high (Hg only, no Cd/La/Y)
2. O6.270Ca1Cu2Ba2Hg1: high (Hg only, no Cd/La/Y)
3. O4.180Cu1Ba2Hg1: high (Hg only, no Cd/La/Y)
4. O4.047Cu1Ba2Hg1: high (Hg only, no Cd/La/Y)
5. O4.066Cu1Ba2Hg1: high (Hg only, no Cd/La/Y)
6. O4.160Cu1Ba2Hg1: high (Hg only, no Cd/La/Y)
7. O8Ca0.850Cu2.200Ba2Tl1.800: high (Tl only, no Cd/La/Y)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: high (Tl only, no Cd/La/Y - Pb not in poison list)
9. O8Cu3Y1Ba1.900La0.100: low (Y and La, no Hg/Tl)
10. O8Cu4Y1Ba1.870La0.130: low (Y and La, no Hg/Tl)

Step D: Among predicted high unmeasurable materials, find fewest elements:
- O4.xxxCu1Ba2Hg1 compositions have 4 elements (fewest)
- Tie break by higher expected Tc: choose O4.160Cu1Ba2Hg1 (oxygen content similar to high-Tc measured compositions)

Final answer written to answer.json.

Budget used: 26 out of 34 measurements (1 additional measurement to verify O4.097Cu1Ba2Hg1).