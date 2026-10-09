# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- High Tc: >= 90K, Low: < 90K

## Step A - Series Peak
Seed series (same element set O-Ca-Cu-Ba-Hg, varying oxygen): lines 1-12 of measurable.txt
Need to measure all 12 and find max Tc (T*).

## Step B - Filter Family B (Hg + exactly 4 elements)
Hg 4-element materials: lines 13-24 (O4.xxxCu1Ba2Hg1 → elements O,Cu,Ba,Hg)
threshold1 = T* - 32, report all with Tc >= threshold1

## Step C - Extrapolate to unmeasurable
Need to induce a multi-factor rule from measurable data.

## Step D - Final pick
Among unmeasurable predicted "high", fewest elements (tie-break: higher expected Tc).

## Measurements Log

### Step A - Series A (O-Ca-Cu-Ba-Hg, 5 elements):
| Composition | Tc | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | high |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | high ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | high ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | high |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | high |

T* = 127.0 K (at O6.210Ca1Cu2Ba2Hg1)
Budget used: 12 calls (10 successful + 2 no sample)

### Step B - Series B (O-Cu-Ba-Hg, 4 elements):
| Composition | Tc | High/Low | >= 95? |
|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | high | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | high | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | high | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | high | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | high | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | high | YES |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | - | - |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | - | - |
| O4.200Cu1Ba2Hg1 | 94.2 | high | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | high | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | high | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | high | NO |

threshold1 = 127.0 - 32 = 95.0
Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}
Budget used: 12 more calls (10 successful + 2 no sample)

### Other materials measured:
| Composition | Tc | High/Low | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | low | O,Cu,Ba,Hg,Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | high | O,Ca,Cu,Ba,Tl (5) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | low | O,Cu,Cd,Ba,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | low | O,Cu,Ba,La (4) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | low | O,Cu,Y,Ba,La (5) |

Budget used: 5 more calls (total 29)

### Step C - Rule Induction:
HIGH materials have: Ca (always high) OR Hg without Tl (always high)
LOW materials: no Ca AND (no Hg OR has Tl)

Rule: HIGH if (contains Ca) OR (contains Hg AND NOT contains Tl)

Verified against all 19 measured compositions: ✓

### Step C Predictions (unmeasurable):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem): has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem): has Ca → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
4. O4.047Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
5. O4.066Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
6. O4.160Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem): has Ca → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem): has Ca → HIGH
9. O8Cu3Y1Ba1.900La0.100 (5 elem): no Ca, no Hg → LOW
10. O8Cu4Y1Ba1.870La0.130 (5 elem): no Ca, no Hg → LOW

### Step D - Final Pick:
HIGH predictions: items 1-8
Fewest elements: 4 → items 3,4,5,6 (all O-Cu-Ba-Hg)
Tie-break by expected Tc:
- O4.160 close to O4.162 (97.4) → ~97.2 K (highest expected)
- O4.180 between O4.162 (97.4) and O4.200 (94.2) → ~96 K
- O4.066 near O4.081 (98.0) but sharp peak → uncertain
- O4.047 furthest from data → uncertain

Final pick: O4.160Cu1Ba2Hg1
