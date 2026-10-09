# Notes on Superconductor Tc Study

## Task Summary
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 18 `measure_tc` calls
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak (COMPLETE)
Measured all 10 members of O6.xxxCa1Cu2Ba2Hg1 series:
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.190: 122.0 K
- O6.210: 127.0 K (PEAK)
- O6.220: 127.0 K (PEAK)
- O6.240: 123.0 K
- O6.300: 100.0 K
- O6.310: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first occurrence of max)

## Step B - Filter Family B (COMPLETE)
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements (O4.xxxCu1Ba2Hg1 series):
- O4.081: 98.0 >= 95.0 ✓
- O4.162: 97.4 >= 95.0 ✓
- O4.150: 97.0 >= 95.0 ✓
- O4.220: 97.0 >= 95.0 ✓
- O4.096: 95.6 >= 95.0 ✓
- O4.097: 95.3 >= 95.0 ✓
- O4.047: 95.0 >= 95.0 ✓
- O4.200: 94.2 < 95.0 ✗
- O4.101: NOT MEASURED (budget exhausted)
- O4.103: NOT MEASURED (budget exhausted)
- O4.085: NOT MEASURED (budget exhausted)

Q1 (confirmed): O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220 Cu1Ba2Hg1

## Step C - Extrapolate (COMPLETE)
Induced rule: **Material is HIGH if it contains Cu, Ba, and at least one of {Hg, Tl, Y}**

This is an interaction rule (multiple factors), not a single-factor rule like "contains Hg".

Predictions:
1. O6.160Ca1Cu2Ba2Hg1 - Cu + Ba + Hg → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - Cu + Ba + Hg → HIGH
3. O4.180Cu1Ba2Hg1 - Cu + Ba + Hg → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Cu + Ba + Tl → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Cu + Tl, no Ba → LOW
6. O8Cu3Y1Ba1.900La0.100 - Cu + Ba + Y → HIGH
7. O8Cu4Y1Ba1.870La0.130 - Cu + Ba + Y → HIGH

## Step D - Final Pick (COMPLETE)
Among HIGH predictions (1-4, 6-7), count elements:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.180Cu1Ba2Hg1 - 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements
6. O8Cu3Y1Ba1.900La0.100 - 5 elements
7. O8Cu4Y1Ba1.870La0.130 - 5 elements

**Final answer: O4.180Cu1Ba2Hg1**

## Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K
- Q1 (qualifying): 7 O4.xxxCu1Ba2Hg1 materials with Tc >= 95.0
- Predictions: 6 HIGH, 1 LOW
- Final: O4.180Cu1Ba2Hg1 (fewest elements among HIGH predictions)
