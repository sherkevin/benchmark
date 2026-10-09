# Superconductor Tc Study Notes

## Overview
- Task: Find optimal superconducting material with limited measurement budget (18 calls)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as >= 90 K
- Budget: 18 measure_tc calls

## Step A - Series Peak
Need to measure homologous doping series (same element set, varying oxygen) to find maximum Tc T*
Series members in data/measurable.txt:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (tied with O6.220Ca1Cu2Ba2Hg1 but O6.210Ca1Cu2Ba2Hg1 appears first in list)

## Step B - Filter Family B
Threshold1 = T* - 32 = 127 - 32 = 95

Materials with exactly 4 elements and containing Hg from measurable.txt:
1. O4.081Cu1Ba2Hg1 - Tc = 98.0 K (≥95) ✓
2. O4.162Cu1Ba2Hg1 - Tc = 97.4 K (≥95) ✓
3. O4.150Cu1Ba2Hg1 - Tc = 97.0 K (≥95) ✓
4. O4.220Cu1Ba2Hg1 - Tc = 97.0 K (≥95) ✓
5. O4.096Cu1Ba2Hg1 - Tc = 95.6 K (≥95) ✓
6. O4.097Cu1Ba2Hg1 - Tc = 95.3 K (≥95) ✓
7. O4.047Cu1Ba2Hg1 - Tc = 95.0 K (≥95) ✓
8. O4.200Cu1Ba2Hg1 - Tc = 94.2 K (<95) ✗
9. O4.101Cu1Ba2Hg1 - Not measured yet (budget exhausted)
10. O4.103Cu1Ba2Hg1 - Not measured yet (budget exhausted)
11. O4.085Cu1Ba2Hg1 - Not measured yet (budget exhausted)

Current budget used: 18/18 (exhausted)

## Step C - Induce Rule
All 18 measured materials have Tc >= 90 (all are "high"). No low-Tc examples measured.

Need to induce rule from patterns in high-Tc materials to predict unmeasurable materials.

Observed patterns:
1. Ca-Cu-Ba-Hg series (5 elements): Tc peaks at O~6.2, decreases on either side
2. Cu-Ba-Hg series (4 elements): Tc generally decreases with increasing O, but not monotonic
3. All measured materials contain Hg

Unmeasurable materials to predict:
1. O6.160Ca1Cu2Ba2Hg1 - In Ca-Cu-Ba-Hg series between O6.140 (105) and O6.190 (122) → Likely high
2. O6.270Ca1Cu2Ba2Hg1 - In Ca-Cu-Ba-Hg series between O6.240 (123) and O6.300 (100) → Could be high (~110-115 range)
3. O4.180Cu1Ba2Hg1 - In Cu-Ba-Hg series between O4.150 (97) and O4.200 (94.2) → Likely high (~95-96)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl, not Hg. No direct comparison. Complex composition.
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Sr, Tl, Pb. No Hg.
6. O8Cu3Y1Ba1.900La0.100 - Contains Y, La. No Hg.
7. O8Cu4Y1Ba1.870La0.130 - Contains Y, La. No Hg.

## Final Answer Summary

Step A:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1 = T* - 32 = 95 K

Step B (Qualifying set Q1):
- O4.081Cu1Ba2Hg1 (98.0 ≥ 95)
- O4.162Cu1Ba2Hg1 (97.4 ≥ 95)
- O4.150Cu1Ba2Hg1 (97.0 ≥ 95)
- O4.220Cu1Ba2Hg1 (97.0 ≥ 95)
- O4.096Cu1Ba2Hg1 (95.6 ≥ 95)
- O4.097Cu1Ba2Hg1 (95.3 ≥ 95)
- O4.047Cu1Ba2Hg1 (95.0 ≥ 95)
- O4.200Cu1Ba2Hg1 (94.2 < 95) - excluded
- O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1 - not measured (budget exhausted)

Step C Predictions:
Based on induced rule: Materials containing Hg, Cu, and Ba with appropriate oxygen stoichiometry have high Tc. Materials without Hg (with Tl, Pb, Sr, Y, La instead) have low Tc.
1. O6.160Ca1Cu2Ba2Hg1 - high (Hg-containing, similar to measured series)
2. O6.270Ca1Cu2Ba2Hg1 - high (Hg-containing, in high-Tc range)
3. O4.180Cu1Ba2Hg1 - high (Hg-containing, in high-Tc range)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - low (Tl instead of Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - low (Tl, Pb, Sr instead of Hg)
6. O8Cu3Y1Ba1.900La0.100 - low (Y, La instead of Hg)
7. O8Cu4Y1Ba1.870La0.130 - low (Y, La instead of Hg)

Step D Final Pick:
Among unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1 (5 elements), O6.270Ca1Cu2Ba2Hg1 (5 elements), O4.180Cu1Ba2Hg1 (4 elements)
- Fewest elements: O4.180Cu1Ba2Hg1 (4 elements)
- Final answer: O4.180Cu1Ba2Hg1

Rule: "Materials containing Hg, Cu, and Ba with oxygen stoichiometry similar to measured high-Tc materials (O~6.0-6.3 for Ca-containing, O~4.0-4.2 for Ca-free) have high Tc (≥90 K). Materials without Hg, or with elements like Tl, Pb, Sr, Y, La instead of Hg, have low Tc."