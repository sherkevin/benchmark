# Notes for Superconductor Tc Study

## Step A — Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1
Need to measure all materials in measurable.txt to find max Tc (T*)

Measurable compositions:
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1
11. O4.081Cu1Ba2Hg1
12. O4.162Cu1Ba2Hg1
13. O4.150Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
15. O4.096Cu1Ba2Hg1
16. O4.097Cu1Ba2Hg1
17. O4.047Cu1Ba2Hg1
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1
22. O6Cu1Ba2Hg1Tl1
23. O10Ca2Cu3Ba2Tl2
24. O6Cu1Cd0.250Ba2Tl1.750
25. O7Cu3Ba2La1
26. O7Cu3Y0.200Ba2La0.800

## Step B — Filter family B
threshold1 = T* - 32
Materials with Hg AND exactly 4 elements, Tc >= threshold1

## Step C — Extrapolate
Unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

Need to induce a rule to predict high (Tc >= 90) vs low (Tc < 90)

## Step D — Final Pick
Among unmeasurable predicted high, pick one with fewest elements (tie-break: higher expected Tc)

---

## Measurements (completed - 18/18 budget used)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 (PEAK)
- O6.220Ca1Cu2Hg1: 127.0 (PEAK tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0
- O4.081Cu1Ba2Hg1: 98.0
- O4.162Cu1Ba2Hg1: 97.4
- O4.150Cu1Ba2Hg1: 97.0
- O4.220Cu1Ba2Hg1: 97.0
- O4.096Cu1Ba2Hg1: 95.6
- O4.097Cu1Ba2Hg1: 95.3
- O4.047Cu1Ba2Hg1: 95.0
- O4.200Cu1Ba2Hg1: 94.2

T* = 127.0 (from O6.210Ca1Cu2Ba2Hg1)
threshold1 = 127.0 - 32 = 95.0

## Step B - Q1 (Hg, 4 elements, Tc >= 95.0)
Materials with Hg and exactly 4 elements: O4.xxxCu1Ba2Hg1 series
Qualifying (Tc >= 95.0):
- O4.047Cu1Ba2Hg1: 95.0 ✓
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
Not qualifying:
- O4.200Cu1Ba2Hg1: 94.2 ✗
Not measured (but likely qualify based on trend):
- O4.085Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1

## Step C - Rule induction
All measured materials contain Cu AND Ba AND Hg, and all have Tc >= 90 (high).
Rule: High if material contains both Cu AND Ba (interaction of two factors).
This distinguishes from single-factor rules like "contains Hg" or "has 5 elements".

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: Cu=yes, Ba=yes → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Cu=yes, Ba=yes → HIGH
3. O4.180Cu1Ba2Hg1: Cu=yes, Ba=yes → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Cu=yes, Ba=yes → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Cu=yes, Ba=no → LOW
6. O8Cu3Y1Ba1.900La0.100: Cu=yes, Ba=yes → HIGH
7. O8Cu4Y1Ba1.870La0.130: Cu=yes, Ba=yes → HIGH

## Step D - Final pick
Among predicted HIGH, find one with fewest elements:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
6. O8Cu3Y1Ba1.900La0.100: 5 elements
7. O8Cu4Y1Ba1.870La0.130: 5 elements

Final answer: O4.180Cu1Ba2Hg1
