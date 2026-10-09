# Notes on Superconductor Tc Study

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high if Tc >= 90 K, otherwise low
- Budget: 34 measure_tc calls

## Step A — Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to measure series members in measurable.txt to find composition with MAXIMUM Tc (T*).

## Step B — Filter Family B
threshold1 = T* - 32
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= threshold1.

## Step C — Extrapolate
Materials in unmeasurable.txt have no physical sample.
Must induce a rule to separate high from low Tc using measurable materials.
Warning: single-factor rules are wrong — need interaction of multiple factors.

## Step D — Final Pick
Among unmeasurable materials predicted high, return the one with FEWEST elements.
Break ties by higher expected Tc.

---

## Progress

### Step A: Finding T*

Measured the O6.xxxCa1Cu2Ba2Hg1 series (homologous series with seed O6.080Ca1Cu2Ba2Hg1):
- O6.080: 92.0 K (high)
- O6.100: 92.5 K (high)
- O6.130: 102.0 K (high)
- O6.140: 105.0 K (high)
- O6.160: no sample
- O6.190: 122.0 K (high)
- O6.210: 127.0 K (high)
- O6.220: 127.0 K (high)
- O6.240: 123.0 K (high)
- O6.270: no sample
- O6.300: 100.0 K (high)
- O6.310: 98.0 K (high)

T* = 127.0 K (maximum)
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first one with max Tc)

### Step B: Q1 (Hg, 4 elements, Tc >= threshold1)

threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials with Hg and exactly 4 elements: O4.xxxCu1Ba2Hg1 series

Measured:
- O4.081: 98.0 >= 95.0 ✓
- O4.162: 97.4 >= 95.0 ✓
- O4.150: 97.0 >= 95.0 ✓
- O4.220: 97.0 >= 95.0 ✓
- O4.096: 95.6 >= 95.0 ✓
- O4.097: 95.3 >= 95.0 ✓
- O4.200: 94.2 < 95.0 ✗
- O4.101: 93.4 < 95.0 ✗
- O4.103: 92.5 < 95.0 ✗
- O4.085: 92.0 < 95.0 ✗
- O4.180: no sample
- O4.047: no sample

Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

### Step C: Induced Rule

Rule: **High if (contains Ca) OR (contains Hg AND has exactly 4 elements)**

Verified against all measured materials - works correctly.

Unmeasurable materials predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Ca -> high
2. O6.270Ca1Cu2Ba2Hg1: has Ca -> high
3. O4.180Cu1Ba2Hg1: no Ca, has Hg, 4 elements -> high
4. O4.047Cu1Ba2Hg1: no Ca, has Hg, 4 elements -> high
5. O4.066Cu1Ba2Hg1: no Ca, has Hg, 4 elements -> high
6. O4.160Cu1Ba2Hg1: no Ca, has Hg, 4 elements -> high
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca -> high
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca -> high
9. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg -> low
10. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg -> low

### Step D: Final Pick

Among high predictions, find the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Tie among 4-element materials: O4.180, O4.047, O4.066, O4.160

Based on measured O4.xxx series, O4.081 has highest Tc (98.0). O4.047 is closest to O4.081 on the low end, so it might have highest expected Tc.

Final answer: O4.047Cu1Ba2Hg1
