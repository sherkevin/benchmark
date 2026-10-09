# Notes for Superconductor Tc Study

## Step A — Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1
Measured all 10 members of O6.xxxCa1Cu2Ba2Hg1 series:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 ← MAX (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 ← also MAX
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0

T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first one with max)

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials with Hg AND exactly 4 elements (O4.xxxCu1Ba2Hg1 series):
- O4.081Cu1Ba2Hg1: 98.0 >= 95 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95 ✓
- O4.047Cu1Ba2Hg1: 95.0 >= 95 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95 ✗
- O4.085Cu1Ba2Hg1: 92.0 < 95 ✗

Q1 = [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]

## Step C — Extrapolate
Measured other materials to find pattern:
- O6Cu1Ba2Hg1Tl1: 0.0 (Has Hg, Has Tl, no Ca) → LOW
- O10Ca2Cu3Ba2Tl2: 118.97 (Has Ca) → HIGH
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 (No Ca, No Hg) → LOW
- O7Cu3Ba2La1: 89.6 (No Ca, No Hg) → LOW
- O7Cu3Y0.200Ba2La0.800: 81.0 (No Ca, No Hg) → LOW

Induced rule: **HIGH if (Contains Ca) OR (Contains Hg AND NOT Contains Tl)**

Predictions for unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: Has Ca → HIGH
- O6.270Ca1Cu2Ba2Hg1: Has Ca → HIGH
- O4.180Cu1Ba2Hg1: Has Hg, no Tl, no Ca → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Has Ca → HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Ca → HIGH
- O8Cu3Y1Ba1.900La0.100: No Ca, No Hg → LOW
- O8Cu4Y1Ba1.870La0.130: No Ca, No Hg → LOW

## Step D — Final Pick
Among HIGH predictions, find one with FEWEST elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Final answer: O4.180Cu1Ba2Hg1

## Budget Used
26 out of 34 calls used.
