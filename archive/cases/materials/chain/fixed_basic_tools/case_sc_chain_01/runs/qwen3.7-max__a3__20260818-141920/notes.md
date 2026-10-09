# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90K, Low: < 90K

## Measurable materials (26 total):
### Series (seed homologous, items 1-10): O6.xxxCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
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

### 4-element Hg materials (items 11-21): O4.xxxCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg)
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

### Other materials (items 22-26):
22. O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)
23. O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl)
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O,Cu,Cd,Ba,Tl)
25. O7Cu3Ba2La1 (4 elements: O,Cu,Ba,La)
26. O7Cu3Y0.200Ba2La0.800 (5 elements: O,Cu,Y,Ba,La)

## Unmeasurable materials (7 total):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

## Budget plan:
- Step A: 10 measurements (series members)
- Step B: 8 measurements (4-element Hg, skip 3)
- Total: 18

## Step A - Measurements (10/18 budget used)

| Composition | Tc (K) | High? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** | Yes |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**Peak: O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K**
(tied with O6.220 at 127.0 K; picking first occurrence)

Dome shape: clear superconducting dome peaking at O~6.21-6.22

## Step B - 4-element Hg materials (8/8 remaining budget used)

threshold1 = 127.0 - 32 = **95.0 K**

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |

Not measured (budget exhausted):
- O4.047Cu1Ba2Hg1: lowest O, likely underdoped → est. ~90-93K → NOT qualifying
- O4.200Cu1Ba2Hg1: above 4.162, trend rising → est. ~96-98K → likely qualifying but unconfirmed
- O4.220Cu1Ba2Hg1: highest O, likely overdoped → est. ~93-95K → NOT qualifying

**Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1}**

## Step C - Rule Induction

ALL 18 measured materials are HIGH (Tc >= 90 K). Lowest measured Tc = 92.0 K.

Key observation: All measured materials contain Hg. The unmeasurable materials 4-7 don't contain Hg.

Unmeasurable analysis:
1. O6.160Ca1Cu2Ba2Hg1: same family as seed, interpolate ~113K → HIGH
2. O6.270Ca1Cu2Ba2Hg1: same family, interpolate ~112K → HIGH
3. O4.180Cu1Ba2Hg1: same family as O4.xxx, interpolate ~97K → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Tl-Ba-Ca-Cu-O family (known high-Tc) → has Ca AND Tl → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl-Sr-Ca-Cu-O with Pb → has Ca AND Tl → HIGH
6. O8Cu3Y1Ba1.900La0.100: Y-Ba-Cu-O with La, no Hg/Ca/Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: Y-Ba-Cu-O with La, more Cu, no Hg/Ca/Tl → LOW

Rule: "A material is high-Tc if it contains Hg, OR if it contains both Ca and Tl"
- Interaction: Ca alone insufficient (need Tl too), Tl alone insufficient (need Ca too)
- "Contains Hg" single-factor wrong on materials 4, 5 (high without Hg)
- "Has 5 elements" single-factor wrong on material 3 (4 elem, high), 6 (5 elem, low), 7 (5 elem, low)

## Step D - Final Pick

High predictions: materials 1-5
Element counts: 5, 5, 4, 5, 6
Fewest: material 3 (O4.180Cu1Ba2Hg1) with 4 elements
Expected Tc ~97 K

**Final: O4.180Cu1Ba2Hg1**
