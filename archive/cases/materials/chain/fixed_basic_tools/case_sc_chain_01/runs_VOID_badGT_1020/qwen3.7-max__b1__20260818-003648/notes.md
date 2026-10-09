# Notes: Superconductor Tc Study

## Seed Material
`O6.080Ca1Cu2Ba2Hg1` — Elements: O, Ca, Cu, Ba, Hg (5 elements)

## Budget Tracking
- Total budget: 34 measure_tc calls
- Used: 25 (10 for O6 series, 10 for O4 series, 5 for other measurables)
- Remaining: 9

## Step A — Series Peak
Homologous series: {O, Ca, Cu, Ba, Hg}, varying oxygen.

| Composition | Tc (K) | Measurable |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | — | NO (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | — | NO (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**Peak: O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K**
(O6.220 ties at 127.0 but O6.210 appears first)

## Step B — Filter Family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

O4.xxxCu1Ba2Hg1 series (elements: O, Cu, Ba, Hg — 4 elements, contains Hg):

| Composition | Tc (K) | >= 95.0? | Measurable |
|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | Yes ✓ | Yes |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes ✓ | Yes |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes ✓ | Yes |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes ✓ | Yes |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes ✓ | Yes |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes ✓ | Yes |
| O4.200Cu1Ba2Hg1 | 94.2 | No | Yes |
| O4.101Cu1Ba2Hg1 | 93.4 | No | Yes |
| O4.103Cu1Ba2Hg1 | 92.5 | No | Yes |
| O4.085Cu1Ba2Hg1 | 92.0 | No | Yes |
| O4.180Cu1Ba2Hg1 | — | — | NO (unmeasurable) |
| O4.047Cu1Ba2Hg1 | — | — | NO (unmeasurable) |

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

## Other Measured Materials
| Composition | Elements | # Elem | Tc (K) | High/Low | Has Hg | Has Ca | Has Tl |
|---|---|---|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl | 5 | 0.0 | Low | Yes | No | Yes |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl | 5 | 118.97 | High | No | Yes | Yes |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl | 5 | 88.0 | Low | No | No | Yes |
| O7Cu3Ba2La1 | O,Cu,Ba,La | 4 | 89.6 | Low | No | No | No |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La | 5 | 81.0 | Low | No | No | No |

## Step C — Rule Induction

### Analysis of all 25 measured materials:
HIGH Tc (>=90 K): All contain either (Hg without Tl) or (Ca).
LOW Tc (<90 K): Either contain Hg+Tl together (without Ca), or lack both Hg and Ca.

### Induced Rule:
**A material is HIGH Tc if: (contains Hg AND does NOT contain Tl) OR (contains Ca)**

Equivalently: High Tc requires a "clean" superconducting element (Hg without Tl interference, OR Ca as charge reservoir).

### Verification against all measured data:
- O6.xxxCa1Cu2Ba2Hg1 (10 items): Hg=yes, Tl=no → High ✓ (all 92-127 K)
- O4.xxxCu1Ba2Hg1 (10 items): Hg=yes, Tl=no → High ✓ (all 92-98 K)
- O10Ca2Cu3Ba2Tl2: Ca=yes → High ✓ (119 K)
- O6Cu1Ba2Hg1Tl1: Hg=yes but Tl=yes (no Ca) → Low ✓ (0 K)
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, no Ca → Low ✓ (88 K)
- O7Cu3Ba2La1: no Hg, no Ca → Low ✓ (89.6 K)
- O7Cu3Y0.200Ba2La0.800: no Hg, no Ca → Low ✓ (81 K)

✅ Rule correctly classifies all 25 measured materials!

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Hg=yes, Tl=no → **HIGH** (5 elements)
2. O6.270Ca1Cu2Ba2Hg1: Hg=yes, Tl=no → **HIGH** (5 elements)
3. O4.180Cu1Ba2Hg1: Hg=yes, Tl=no → **HIGH** (4 elements)
4. O4.047Cu1Ba2Hg1: Hg=yes, Tl=no → **HIGH** (4 elements)
5. O4.066Cu1Ba2Hg1: Hg=yes, Tl=no → **HIGH** (4 elements)
6. O4.160Cu1Ba2Hg1: Hg=yes, Tl=no → **HIGH** (4 elements)
7. O8Ca0.850Cu2.200Ba2Tl1.800: Ca=yes → **HIGH** (5 elements: O,Ca,Cu,Ba,Tl)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca=yes → **HIGH** (6 elements: O,Ca,Cu,Sr,Tl,Pb)
9. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → **LOW** (5 elements: O,Cu,Y,Ba,La)
10. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → **LOW** (5 elements: O,Cu,Y,Ba,La)

## Step D — Final Pick
Among HIGH predictions, sorted by element count:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4: Four-way tie among O4 series members.
Tie-break by higher expected Tc:
- O4.160Cu1Ba2Hg1: near O4.162 (97.4) → expected ~97 K
- O4.180Cu1Ba2Hg1: between O4.162 (97.4) and O4.200 (94.2) → expected ~96 K
- O4.066Cu1Ba2Hg1: near O4.081 (98.0) → expected ~96 K
- O4.047Cu1Ba2Hg1: further from measured data → expected ~95 K

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ~97 K)
