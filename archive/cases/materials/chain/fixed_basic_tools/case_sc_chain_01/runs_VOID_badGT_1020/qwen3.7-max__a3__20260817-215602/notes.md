# Notes - Superconductor Tc Study

## Seed Material
`O6.080Ca1Cu2Ba2Hg1`

## Task Overview
- Step A: Find peak Tc in seed's homologous series (same elements, varying O)
- Step B: Filter family B (Hg + exactly 4 elements) with Tc >= T* - 32
- Step C: Induce rule for high/low Tc, predict for unmeasurable materials
- Step D: Among predicted-high unmeasurable, pick fewest elements

## Measurable Materials (29 total)
### Series 1 (O6.xxxCa1Cu2Ba2Hg1) - same element set as seed, 5 elements:
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

### Series 2 (O4.xxxCu1Ba2Hg1) - 4 elements, contains Hg:
13. O4.081Cu1Ba2Hg1
14. O4.162Cu1Ba2Hg1
15. O4.150Cu1Ba2Hg1
16. O4.220Cu1Ba2Hg1
17. O4.096Cu1Ba2Hg1
18. O4.097Cu1Ba2Hg1
19. O4.180Cu1Ba2Hg1
20. O4.047Cu1Ba2Hg1
21. O4.200Cu1Ba2Hg1
22. O4.101Cu1Ba2Hg1
23. O4.103Cu1Ba2Hg1
24. O4.085Cu1Ba2Hg1

### Others:
25. O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)
26. O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl)
27. O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O,Cu,Cd,Ba,Tl)
28. O7Cu3Ba2La1 (4 elements: O,Cu,Ba,La)
29. O7Cu3Y0.200Ba2La0.800 (5 elements: O,Cu,Y,Ba,La)

## Unmeasurable Materials (10 total)
1. O6.160Ca1Cu2Ba2Hg1 (also in measurable!)
2. O6.270Ca1Cu2Ba2Hg1 (also in measurable!)
3. O4.180Cu1Ba2Hg1 (also in measurable!)
4. O4.047Cu1Ba2Hg1 (also in measurable!)
5. O4.066Cu1Ba2Hg1
6. O4.160Cu1Ba2Hg1
7. O8Ca0.850Cu2.200Ba2Tl1.800
8. O7Ca1Cu2Sr2Tl0.500Pb0.500
9. O8Cu3Y1Ba1.900La0.100
10. O8Cu4Y1Ba1.870La0.130

## Step A Measurements — Homologous Series (O6.xxxCa1Cu2Ba2Hg1)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | — | no sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← MAX |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured ← MAX |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | — | no sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)
Budget used: 10 (2 no-sample didn't count)

## Step B — Hg + 4 elements family (O4.xxxCu1Ba2Hg1)
threshold1 = 127.0 - 32 = 95.0

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.180Cu1Ba2Hg1 | — | no sample |
| O4.047Cu1Ba2Hg1 | — | no sample |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |

**Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**
Budget used: 20 (10 more, 2 no-sample)

## Other Measurable Materials
| Composition | Tc (K) | High/Low | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW | O,Cu,Ba,Hg,Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | HIGH | O,Ca,Cu,Ba,Tl (5) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | LOW | O,Cu,Cd,Ba,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | LOW | O,Cu,Ba,La (4) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | LOW | O,Cu,Y,Ba,La (5) |
Budget used: 25

## Step C — Rule Induction

### Summary of all measured data:
HIGH (Tc >= 90):
- O6.xxxCa1Cu2Ba2Hg1 (10 measured): all HIGH (92-127 K). Elements: O,Ca,Cu,Ba,Hg
- O4.xxxCu1Ba2Hg1 (10 measured): all HIGH (92-98 K). Elements: O,Cu,Ba,Hg
- O10Ca2Cu3Ba2Tl2: HIGH (118.97 K). Elements: O,Ca,Cu,Ba,Tl

LOW (Tc < 90):
- O6Cu1Ba2Hg1Tl1: LOW (0 K). Elements: O,Cu,Ba,Hg,Tl
- O6Cu1Cd0.250Ba2Tl1.750: LOW (88 K). Elements: O,Cu,Cd,Ba,Tl
- O7Cu3Ba2La1: LOW (89.6 K). Elements: O,Cu,Ba,La
- O7Cu3Y0.200Ba2La0.800: LOW (81 K). Elements: O,Cu,Y,Ba,La

### Induced Rule:
**HIGH if (contains Hg AND does NOT contain Tl) OR (contains Ca)**

Verification:
- O6.xxxCa1Cu2Ba2Hg1: Hg✓, no Tl✓ → HIGH ✓ (also has Ca)
- O4.xxxCu1Ba2Hg1: Hg✓, no Tl✓ → HIGH ✓
- O10Ca2Cu3Ba2Tl2: no Hg → first fails; Ca✓ → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Hg✓ but Tl✓ → first fails; no Ca → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Hg → first fails; no Ca → LOW ✓
- O7Cu3Ba2La1: no Hg → first fails; no Ca → LOW ✓
- O7Cu3Y0.200Ba2La0.800: no Hg → first fails; no Ca → LOW ✓

This is a multi-factor interaction rule (involves Hg, Tl, and Ca), consistent with the task's warning.

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
4. O4.047Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
5. O4.066Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
6. O4.160Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg): Hg✓ no Tl✓ → **HIGH**
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl): Ca✓ → **HIGH**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb): Ca✓ → **HIGH**
9. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La): no Hg, no Ca → **LOW**
10. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La): no Hg, no Ca → **LOW**

## Step D — Final Pick
Predicted HIGH unmeasurable materials:
- #1: 5 elements
- #2: 5 elements
- #3: 4 elements
- #4: 4 elements
- #5: 4 elements
- #6: 4 elements
- #7: 5 elements
- #8: 6 elements

Fewest elements = 4: candidates #3, #4, #5, #6 (all O4.xxxCu1Ba2Hg1)

Estimated Tc for tie-breaking (based on O4.xxxCu1Ba2Hg1 measured trend):
- O4.180: between O4.162(97.4) and O4.200(94.2) → ~95-96
- O4.047: below O4.081(98.0), extrapolation uncertain → ~96-99?
- O4.066: between O4.047 and O4.081(98.0) → ~96-98
- O4.160: close to O4.162(97.4) and O4.150(97.0) → ~97

O4.160Cu1Ba2Hg1 is most confidently near ~97 K (closest to measured peaks).

**Final pick: O4.160Cu1Ba2Hg1**
