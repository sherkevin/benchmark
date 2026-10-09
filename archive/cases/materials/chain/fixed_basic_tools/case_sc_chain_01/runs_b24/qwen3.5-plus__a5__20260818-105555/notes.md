# Notes for Superconductor Tc Study

## Task Summary
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K
- Budget: 24 measure_tc calls

## Step A — Series Peak
Measure all members in measurable.txt to find composition with MAX Tc (T*)

## Step B — Filter Family B
threshold1 = T* - 32
Find all materials with Hg and exactly 4 elements where Tc >= threshold1

## Step C — Extrapolate
Induce rule to separate high/low Tc for unmeasurable materials

## Step D — Final Pick
Among predicted high unmeasurables, pick one with fewest elements (tie-break: higher expected Tc)

---

## Measurements (24 total - budget exhausted)

| Composition | Tc (K) | High/Low |
|-------------|--------|----------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | High |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | High |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | High |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | High |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | High |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | High (PEAK) |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | High |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | High |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | High |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | High |
| O4.081Cu1Ba2Hg1 | 98.0 | High |
| O4.162Cu1Ba2Hg1 | 97.4 | High |
| O4.150Cu1Ba2Hg1 | 97.0 | High |
| O4.220Cu1Ba2Hg1 | 97.0 | High |
| O4.096Cu1Ba2Hg1 | 95.6 | High |
| O4.097Cu1Ba2Hg1 | 95.3 | High |
| O4.047Cu1Ba2Hg1 | 95.0 | High |
| O4.200Cu1Ba2Hg1 | 94.2 | High |
| O4.101Cu1Ba2Hg1 | 93.4 | High |
| O4.103Cu1Ba2Hg1 | 92.5 | High |
| O4.085Cu1Ba2Hg1 | 92.0 | High |
| O6Cu1Ba2Hg1Tl1 | 0.0 | Low |
| O10Ca2Cu3Ba2Tl2 | 118.97 | High |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | Low |

## Step A Results
- T* (max Tc) = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B Results
- threshold1 = 127.0 - 32 = 95.0
- Materials with Hg + exactly 4 elements + Tc >= 95.0:
  - O4.081Cu1Ba2Hg1 (98.0)
  - O4.162Cu1Ba2Hg1 (97.4)
  - O4.150Cu1Ba2Hg1 (97.0)
  - O4.220Cu1Ba2Hg1 (97.0)
  - O4.096Cu1Ba2Hg1 (95.6)
  - O4.097Cu1Ba2Hg1 (95.3)
  - O4.047Cu1Ba2Hg1 (95.0)

## Step C - Induced Rule
**Rule: High Tc if (contains Ca) OR (contains Hg AND NOT contains Tl)**

Analysis:
- All materials with Ca are high (O10Ca2Cu3Ba2Tl2: 118.97)
- Materials with Hg but no Tl are high (all O4.xxxCu1Ba2Hg1 series: 92-98)
- Material with Hg AND Tl is low (O6Cu1Ba2Hg1Tl1: 0.0)
- Material with neither Hg nor Ca is low (O6Cu1Cd0.250Ba2Tl1.750: 88.0)

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - has Ca → HIGH
3. O4.180Cu1Ba2Hg1 - has Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100 - no Ca, no Hg → LOW
7. O8Cu4Y1Ba1.870La0.130 - no Ca, no Hg → LOW

## Step D - Final Pick
Among HIGH predictions, find fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1**
