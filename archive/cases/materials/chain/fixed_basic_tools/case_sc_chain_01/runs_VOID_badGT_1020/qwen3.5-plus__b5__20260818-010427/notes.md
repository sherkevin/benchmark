# Superconductor Tc Study - Notes

## Step A — Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1
Task: Measure series members in data/measurable.txt to find MAX Tc (T*)

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Hg1)

Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0
- O4.081Cu1Ba2Hg1: 98.0
- O4.162Cu1Ba2Hg1: 97.4
- O4.150Cu1Ba2Hg1: 97.0
- O4.220Cu1Ba2Hg1: 97.0
- O4.096Cu1Ba2Hg1: 95.6
- O4.097Cu1Ba2Hg1: 95.3
- O4.200Cu1Ba2Hg1: 94.2
- O4.101Cu1Ba2Hg1: 93.4
- O4.103Cu1Ba2Hg1: 92.5
- O4.085Cu1Ba2Hg1: 92.0
- O6Cu1Ba2Hg1Tl1: 0.0
- O10Ca2Cu3Ba2Tl2: 118.97
- O6Cu1Cd0.250Ba2Tl1.750: 88.0
- O7Cu3Ba2La1: 89.6
- O7Cu3Y0.200Ba2La0.800: 81.0

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials with Hg and exactly 4 elements, Tc >= 95.0 (Q1):
- O4.081Cu1Ba2Hg1 (98.0) ✓
- O4.162Cu1Ba2Hg1 (97.4) ✓
- O4.150Cu1Ba2Hg1 (97.0) ✓
- O4.220Cu1Ba2Hg1 (97.0) ✓
- O4.096Cu1Ba2Hg1 (95.6) ✓
- O4.097Cu1Ba2Hg1 (95.3) ✓

## Step C — Extrapolate
Induced rule: **High if (has Hg AND no Tl) OR (has Ca)**

Unmeasurable predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Ca → HIGH
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → HIGH (4 elements)
4. O4.047Cu1Ba2Hg1: has Hg, no Tl → HIGH (4 elements)
5. O4.066Cu1Ba2Hg1: has Hg, no Tl → HIGH (4 elements)
6. O4.160Cu1Ba2Hg1: has Hg, no Tl → HIGH (4 elements)
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH (5 elements)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH (6 elements)
9. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg, no Tl → LOW
10. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg, no Tl → LOW

## Step D — Final Pick
HIGH predictions with 4 elements (fewest):
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1

Tie-break by expected Tc: O4.066Cu1Ba2Hg1 is closest to measured peak O4.081 (98.0)
Final answer: O4.066Cu1Ba2Hg1
