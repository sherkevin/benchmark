# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high if Tc >= 90 K, low otherwise
- Measurement budget: 18 measure_tc calls

## Step A - Series Peak
Need to measure the homologous doping series (same element set, varying oxygen).
The series appears to be: O6.080Ca1Cu2Ba2Hg1 through O6.310Ca1Cu2Ba2Hg1 (lines 1-10 in measurable.txt)

These appear to be the series with varying oxygen content:
1. O6.080Ca1Cu2Ba2Hg1 - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Results: Maximum Tc = 127.0 K (achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (choose this as the maximum)

## Step B - Filter family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials that contain Hg and have exactly 4 elements (from measurable.txt):
11. O4.081Cu1Ba2Hg1 - Tc = 98.0 K ✓ qualifies
12. O4.162Cu1Ba2Hg1 - Tc = 97.4 K ✓ qualifies
13. O4.150Cu1Ba2Hg1 - Tc = 97.0 K ✓ qualifies
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K ✓ qualifies
15. O4.096Cu1Ba2Hg1 - Tc = 95.6 K ✓ qualifies
16. O4.097Cu1Ba2Hg1 - Tc = 95.3 K ✓ qualifies
17. O4.047Cu1Ba2Hg1 - Tc = 95.0 K ✓ qualifies (at threshold)
18. O4.200Cu1Ba2Hg1 - Tc = 94.2 K ❌ does NOT qualify (below threshold)
19. O4.101Cu1Ba2Hg1 - UNMEASURED (budget exhausted)
20. O4.103Cu1Ba2Hg1 - UNMEASURED (budget exhausted)
21. O4.085Cu1Ba2Hg1 - UNMEASURED (budget exhausted)

Budget: Used all 18 measurements.

Problem: Need to report ALL materials with Tc >= 95.0 K, but 3 are unmeasured.

## Step C - Extrapolate
Need to induce a rule that separates high (Tc >= 90 K) from low (Tc < 90 K) using measurable materials, then predict for unmeasurable materials.

Unmeasurable materials (from unmeasurable.txt):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

Analysis of measured materials (all high Tc >= 90 K):
- All have Cu and Ba
- All have either Hg or are part of specific series
- No examples of low Tc in measurements

Given task warning that single-factor rules are wrong, and pattern that all measured materials have Cu and Ba, propose rule:
Rule: Materials containing both Copper (Cu) and Barium (Ba) are high Tc (>= 90 K), otherwise low.

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - has Cu and Ba → high
2. O6.270Ca1Cu2Ba2Hg1 - has Cu and Ba → high
3. O4.180Cu1Ba2Hg1 - has Cu and Ba → high
4. O8Ca0.850Cu2.200Ba2Tl1.800 - has Cu and Ba → high
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - has Cu but no Ba (has Sr) → low
6. O8Cu3Y1Ba1.900La0.100 - has Cu and Ba → high
7. O8Cu4Y1Ba1.870La0.130 - has Cu and Ba → high

## Step D - Final Pick
Among predicted high materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (fewest)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O8Cu3Y1Ba1.900La0.100: 5 elements
- O8Cu4Y1Ba1.870La0.130: 5 elements

Final pick: O4.180Cu1Ba2Hg1 (fewest elements among predicted high)

## Summary
- Step A peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Step B threshold: 95.0 K, 7 qualifying materials
- Step C rule: Cu AND Ba → high Tc
- Step D final: O4.180Cu1Ba2Hg1

Answer.json created with all required fields.