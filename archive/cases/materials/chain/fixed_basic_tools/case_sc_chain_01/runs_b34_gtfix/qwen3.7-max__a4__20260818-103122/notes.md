# Notes for Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- High = Tc >= 90K, Low = Tc < 90K

## Step A: Measure all measurable materials, find peak Tc (T*)
### Measurable materials (26 total):
**Series 1: O6.xxxCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)**
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

**Series 2: O4.xxxCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg)**
11. O4.081Cu1Ba2Hg1
12. O4.162Cu1Ba2Hg1
13. O4.150Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
15. O4.096Cu1Ba2Hg1
16. O4.097Cu1Ba2Hg1
17. O4.047Cu1Ba2Hg1
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1

**Other:**
22. O6Cu1Ba2Hg1Tl1 (5 elements)
23. O10Ca2Cu3Ba2Tl2 (5 elements)
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements)
25. O7Cu3Ba2La1 (4 elements)
26. O7Cu3Y0.200Ba2La0.800 (5 elements)

### Unmeasurable materials (7 total):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

## Measurements (26/34 budget used)

### Series 1: O6.xxxCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
| Composition | Tc (K) | High? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | ✓ |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | ✓ |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | ✓ |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | ✓ |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | ✓ |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | ✓ ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | ✓ ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | ✓ |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | ✓ |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | ✓ |

### Series 2: O4.xxxCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg)
| Composition | Tc (K) | High? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.047Cu1Ba2Hg1 | 95.0 | ✓ |
| O4.200Cu1Ba2Hg1 | 94.2 | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✓ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✓ |

### Other materials
| Composition | Tc (K) | High? | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | ✗ | O,Cu,Ba,Hg,Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | ✓ | O,Ca,Cu,Ba,Tl (5) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | ✗ | O,Cu,Cd,Ba,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | ✗ | O,Cu,Ba,La (4) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | ✗ | O,Cu,Y,Ba,La (5) |

## Step A Result
- T* = 127.0 K (peak composition: O6.210Ca1Cu2Ba2Hg1, tied with O6.220Ca1Cu2Ba2Hg1)

## Step B Result
- threshold1 = 127.0 - 32 = 95.0 K
- Filter: contains Hg AND exactly 4 elements
- All O4.xxxCu1Ba2Hg1 materials qualify (4 elements, contain Hg)
- Q1 = those with Tc >= 95.0:
  1. O4.081Cu1Ba2Hg1: 98.0
  2. O4.162Cu1Ba2Hg1: 97.4
  3. O4.150Cu1Ba2Hg1: 97.0
  4. O4.220Cu1Ba2Hg1: 97.0
  5. O4.096Cu1Ba2Hg1: 95.6
  6. O4.097Cu1Ba2Hg1: 95.3
  7. O4.047Cu1Ba2Hg1: 95.0

## Step C: Rule Induction

### HIGH Tc materials (Tc >= 90):
- O6.xxxCa1Cu2Ba2Hg1: has Ca, has Hg, no Tl
- O4.xxxCu1Ba2Hg1: no Ca, has Hg, no Tl
- O10Ca2Cu3Ba2Tl2: has Ca, no Hg, has Tl

### LOW Tc materials (Tc < 90):
- O6Cu1Ba2Hg1Tl1: no Ca, has Hg, has Tl (0 K!)
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, no Hg, has Tl (88 K)
- O7Cu3Ba2La1: no Ca, no Hg, no Tl (89.6 K)
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg, no Tl (81 K)

### Pattern analysis:
Single-factor rules fail:
- "Contains Hg" → O6Cu1Ba2Hg1Tl1 has Hg but is LOW
- "Has 5 elements" → O7Cu3Ba2La1 has 4 elements but is LOW, O4.xxxCu1Ba2Hg1 has 4 elements and is HIGH
- "Contains Ca" → O4.xxxCu1Ba2Hg1 is HIGH but has no Ca
- "Contains Tl" → O10Ca2Cu3Ba2Tl2 has Tl and is HIGH

**Induced Rule: A material is HIGH if it contains Ca, OR (contains Hg AND does NOT contain Tl).**

This is an interaction between three factors: Ca presence, Hg presence, and Tl presence.

### Verification against all measurable data:
- All O6.xxxCa1Cu2Ba2Hg1: Ca → HIGH ✓
- All O4.xxxCu1Ba2Hg1: Hg + no Tl → HIGH ✓
- O10Ca2Cu3Ba2Tl2: Ca → HIGH ✓
- O6Cu1Ba2Hg1Tl1: no Ca, Hg + Tl → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, no Hg → LOW ✓
- O7Cu3Ba2La1: no Ca, no Hg → LOW ✓
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg → LOW ✓

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Ca → HIGH
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → LOW
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → LOW

## Step D: Final Pick
HIGH unmeasurable materials by element count:
1. O4.180Cu1Ba2Hg1: 4 elements (O,Cu,Ba,Hg) ← FEWEST
2. O6.160Ca1Cu2Ba2Hg1: 5 elements
3. O6.270Ca1Cu2Ba2Hg1: 5 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among HIGH predictions)
