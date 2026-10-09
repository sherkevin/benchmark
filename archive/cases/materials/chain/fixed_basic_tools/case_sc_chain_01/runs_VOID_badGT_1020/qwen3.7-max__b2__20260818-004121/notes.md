# Notes for Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90K, Low Tc: < 90K
- Budget: 34 measure_tc calls

## Data Files
- measurable.txt: 29 materials
- unmeasurable.txt: 10 materials

## Step A — Series Peak (SEED SERIES: O-Ca-Cu-Ba-Hg, 5 elements)
| Composition | Tc (K) | Notes |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | high |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | high |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** | **PEAK** |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | also peak (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | high |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | high |

**T* = 127.0 K, Peak = O6.210Ca1Cu2Ba2Hg1**

## Step B — O-Cu-Ba-Hg series (4 elements, contains Hg)
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | - |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | - |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |

**threshold1 = 127.0 - 32 = 95.0 K**
**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

## Misc Measured
| Composition | Tc (K) | High/Low | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW | O,Cu,Ba,Hg,Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | HIGH | O,Ca,Cu,Ba,Tl (5) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | LOW | O,Cu,Cd,Ba,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | LOW | O,Cu,Ba,La (4) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | LOW | O,Cu,Y,Ba,La (5) |

## Step C — Rule Induction

### Analysis of High vs Low
HIGH (Tc >= 90):
- All O-Ca-Cu-Ba-Hg (10): contain Hg, no Tl, have Ca
- All O-Cu-Ba-Hg (10): contain Hg, no Tl, no Ca
- O10Ca2Cu3Ba2Tl2: contains Ca and Tl, no Hg

LOW (Tc < 90):
- O6Cu1Ba2Hg1Tl1 (0K): has Hg AND Tl, no Ca
- O6Cu1Cd0.250Ba2Tl1.750 (88K): has Tl, no Hg, no Ca
- O7Cu3Ba2La1 (89.6K): no Hg, no Tl, no Ca
- O7Cu3Y0.200Ba2La0.800 (81K): no Hg, no Tl, no Ca

### Induced Rule
**A material is HIGH Tc if and only if: (contains Hg AND does NOT contain Tl) OR (contains Ca AND contains at least one of {Hg, Tl})**

Equivalently: High Tc requires a specific charge-reservoir interaction:
- Hg-based cuprates work well UNLESS Tl is also present (competing reservoirs)
- Ca enables Tl-based cuprates to be high-Tc (Ca acts as interlayer spacer)
- Without Hg, Tl, or Ca in the right combination → low Tc

### Predictions for Unmeasurable Materials
1. O6.160Ca1Cu2Ba2Hg1 (5 elem): Hg, no Tl → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 (5 elem): Hg, no Tl → **HIGH**
3. O4.180Cu1Ba2Hg1 (4 elem): Hg, no Tl → **HIGH**
4. O4.047Cu1Ba2Hg1 (4 elem): Hg, no Tl → **HIGH**
5. O4.066Cu1Ba2Hg1 (4 elem): Hg, no Tl → **HIGH**
6. O4.160Cu1Ba2Hg1 (4 elem): Hg, no Tl → **HIGH**
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem): Ca+Tl → **HIGH**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem): Ca+Tl → **HIGH**
9. O8Cu3Y1Ba1.900La0.100 (5 elem): no Hg, no Ca, no Tl → **LOW**
10. O8Cu4Y1Ba1.870La0.130 (5 elem): no Hg, no Ca, no Tl → **LOW**

## Step D — Final Pick
Predicted HIGH unmeasurables by element count:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4. Tie-break by higher expected Tc.
Based on O-Cu-Ba-Hg series trend (nearby: O4.150=97.0, O4.162=97.4):
- O4.160Cu1Ba2Hg1 → expected ~97 K (highest among 4-elem)

**FINAL: O4.160Cu1Ba2Hg1**

## Budget
Used: 25 of 34 (4 no-sample calls didn't count)
