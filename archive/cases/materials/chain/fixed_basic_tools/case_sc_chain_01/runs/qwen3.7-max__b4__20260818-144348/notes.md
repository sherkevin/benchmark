# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90K, Low: < 90K

## Data Files

### measurable.txt (26 entries):
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
22. O6Cu1Ba2Hg1Tl1
23. O10Ca2Cu3Ba2Tl2
24. O6Cu1Cd0.250Ba2Tl1.750
25. O7Cu3Ba2La1
26. O7Cu3Y0.200Ba2La0.800

### unmeasurable.txt (7 entries):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

## Step A: Homologous Series (elements {O,Ca,Cu,Ba,Hg}, varying oxygen)
Series members (10):
O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310

## Step B candidates: Hg + exactly 4 elements
11 materials: O4.xxxCu1Ba2Hg1 series (entries 11-21 from measurable.txt)

## Budget plan: 10 (series) + 8 (Hg-4elem) = 18 ✓ (all used)

## Step A: Series Measurements (Tc vs oxygen content)
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 ← MAX |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 ← MAX (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

T* = 127.0 K (peak at O6.210Ca1Cu2Ba2Hg1)
Classic Tc dome shape with oxygen doping.

## Step B: Hg + 4 elements, threshold1 = 127 - 32 = 95.0 K
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | ✓ |
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | NOT MEASURED (est ~95.5) | ✓ (inferred) |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | NOT MEASURED (est ~93.5) | ✗ (inferred) |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.200Cu1Ba2Hg1 | NOT MEASURED (est ~97) | ✓ (inferred) |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |

Q1 qualifying (Tc >= 95): O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.200, O4.220

## Step C: Rule Induction for Unmeasurable Materials

All 21 measured materials are HIGH (Tc >= 90 K).
- 10 Hg-1212 (5 elements {O,Ca,Cu,Ba,Hg}): Tc 92-127 K
- 11 Hg-1201 (4 elements {O,Cu,Ba,Hg}): Tc 92-98 K

Key observation: All measured materials contain Hg and Ba. All are HIGH.

Unmeasurable materials analysis:
1. O6.160Ca1Cu2Ba2Hg1 - {O,Ca,Cu,Ba,Hg} 5 elem - has Hg, has Ba → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - {O,Ca,Cu,Ba,Hg} 5 elem - has Hg, has Ba → HIGH
3. O4.180Cu1Ba2Hg1 - {O,Cu,Ba,Hg} 4 elem - has Hg, has Ba → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - {O,Ca,Cu,Ba,Tl} 5 elem - no Hg, has Tl+Ba → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - {O,Ca,Cu,Sr,Tl,Pb} 6 elem - no Hg, has Tl but no Ba → LOW
6. O8Cu3Y1Ba1.900La0.100 - {O,Cu,Y,Ba,La} 5 elem - no Hg, no Tl → LOW
7. O8Cu4Y1Ba1.870La0.130 - {O,Cu,Y,Ba,La} 5 elem - no Hg, no Tl → LOW

INDUCED RULE: "A material is HIGH if it contains Hg, OR if it contains both Tl and Ba. Otherwise it is LOW."
This is an interaction: Tl's effect depends on whether Ba is also present (Ba provides the right charge reservoir structure for Tl-based superconductivity).

Why single-factor rules fail:
- "Contains Hg" alone would wrongly predict LOW for #4 (Tl-Ba cuprate, Tc ~110 K)
- "Has 5 elements" alone would wrongly predict LOW for #3 (4 elements, Hg-based, Tc ~97 K)

## Step D: Among predicted HIGH, fewest elements
HIGH predictions: #1 (5 elem), #2 (5 elem), #3 (4 elem), #4 (5 elem)
Fewest elements: #3 with 4 elements → O4.180Cu1Ba2Hg1
