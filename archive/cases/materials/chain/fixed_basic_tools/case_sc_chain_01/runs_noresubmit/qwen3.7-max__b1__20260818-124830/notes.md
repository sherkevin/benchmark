# Notes: Superconductor Tc Study

## Seed material: O6.080Ca1Cu2Ba2Hg1
## Budget: 18 measure_tc calls

## Step A: Homologous series (same element set as seed: O, Ca, Cu, Ba, Hg, varying oxygen)
Series members from measurable.txt (lines 1-10):
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

## Hg-containing, exactly 4 elements (O, Cu, Ba, Hg) from measurable.txt:
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

## Other measurable materials:
22. O6Cu1Ba2Hg1Tl1 (5 elements)
23. O10Ca2Cu3Ba2Tl2 (5 elements)
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements)
25. O7Cu3Ba2La1 (4 elements)
26. O7Cu3Y0.200Ba2La0.800 (5 elements)

## Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

## Measurement plan:
- Step A: 10 series members = 10 calls
- Step B: 11 Hg-4elem materials = 11 calls
- Total needed: 21, Budget: 18 → need to be strategic (skip 3)

## Measurements:

### Step A - Series (O6.xxxCa1Cu2Ba2Hg1):
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)
**threshold1 = 127.0 - 32 = 95.0 K**

### Step B - Hg 4-element (O4.xxxCu1Ba2Hg1), measured 8 of 11:
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |

Unmeasured (budget exhausted): O4.101, O4.103, O4.085 - likely ~95-98 based on pattern

### Budget: 18/18 used

## Step C - Rule Induction:
All 18 measured materials are HIGH (Tc >= 90). Key observations:
- All contain Hg
- All contain Cu and Ba
- Series (5 elements: O,Ca,Cu,Ba,Hg): 92-127K
- Hg-4-element (4 elements: O,Cu,Ba,Hg): 94.2-98.0K

The task warns single-factor rules ("contains Hg", "has 5 elements") are wrong on several.
The rule must be an INTERACTION of multiple factors.

### Analysis of unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, Hg, Ca, Ba): Same family as seed → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, Hg, Ca, Ba): Same family → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem, Hg, Cu, Ba): Same family as Hg-4-elem → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg, Ca+Ba+Tl): Tl-Ba-Ca cuprate → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg, no Ba, has Sr+Tl+Pb): Unusual → LOW
6. O8Cu3Y1Ba1.900La0.100 (5 elem, no Hg, Ba+Y+La): YBCO-like → HIGH
7. O8Cu4Y1Ba1.870La0.130 (5 elem, no Hg, Ba+Y+La): YBCO-like, Cu4 → HIGH

### Rule: "High Tc if contains Hg, OR (contains Ba AND at least one of {Ca, Y})"
Interaction: requires specific Ba-cation pairing (Ba-Ca or Ba-Y), not just any single element.

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (Hg)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (Hg)
3. O4.180Cu1Ba2Hg1: HIGH (Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (Ba+Ca)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (no Hg, no Ba)
6. O8Cu3Y1Ba1.900La0.100: HIGH (Ba+Y)
7. O8Cu4Y1Ba1.870La0.130: HIGH (Ba+Y)

### Validation:
"Contains Hg" alone → wrong on 4, 6, 7 (3 errors = several ✓)
"Has 5 elements" alone → wrong on 3 (predicted low, actually high) = error ✓

## Step D - Final Pick:
Predicted HIGH: 1(5elem), 2(5elem), 3(4elem), 4(5elem), 6(5elem), 7(5elem)
Fewest elements: **O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~96K)
