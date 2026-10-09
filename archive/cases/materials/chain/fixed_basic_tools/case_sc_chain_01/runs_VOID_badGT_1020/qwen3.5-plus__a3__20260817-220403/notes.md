# Notes for Superconductor Tc Study

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: 34 measure_tc calls (used 25)

## Step A — Find series peak (T*)
Measured the O6.xxxCa1Cu2Ba2Hg1 series:
- O6.080: 92.0, O6.100: 92.5, O6.130: 102.0, O6.140: 105.0
- O6.190: 122.0, O6.210: 127.0, O6.220: 127.0, O6.240: 123.0
- O6.300: 100.0, O6.310: 98.0
- O6.160 and O6.270: no sample (unmeasurable)

**Peak T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (first occurrence)

## Step B — Filter family B
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Materials with Hg AND exactly 4 elements: O4.xxxCu1Ba2Hg1 series

Measured O4.xxxCu1Ba2Hg1:
- O4.081: 98.0 ✓, O4.162: 97.4 ✓, O4.150: 97.0 ✓, O4.220: 97.0 ✓
- O4.096: 95.6 ✓, O4.097: 95.3 ✓, O4.180: no sample, O4.047: no sample
- O4.200: 94.2 ✗, O4.101: 93.4 ✗, O4.103: 92.5 ✗, O4.085: 92.0 ✗

**Q1 = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"]**

## Step C — Induced Rule
**Rule: A material is HIGH Tc if it contains Hg (without Tl) OR contains both Ca AND Tl (without La and without Y).**

Equivalently: **LOW if contains La OR contains Y OR (contains Hg AND Tl) OR (neither Hg nor Ca+Tl combination)**

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: has Hg, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Hg, no Tl → HIGH
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → HIGH
4. O4.047Cu1Ba2Hg1: has Hg, no Tl → HIGH
5. O4.066Cu1Ba2Hg1: has Hg, no Tl → HIGH
6. O4.160Cu1Ba2Hg1: has Hg, no Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca+Tl, no La/Y → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca+Tl, no La/Y → HIGH
9. O8Cu3Y1Ba1.900La0.100: has Y, La → LOW
10. O8Cu4Y1Ba1.870La0.130: has Y, La → LOW

## Step D — Final Pick
Among HIGH predictions, find one with FEWEST elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Four materials tie with 4 elements. Based on expected Tc (comparing to measured O4.xxx series), O4.160Cu1Ba2Hg1 is near O4.162 (97.4) and O4.150 (97.0), suggesting ~97 K expected Tc.

**Final answer: O4.160Cu1Ba2Hg1**
