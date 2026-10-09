# Notes on Superconductor Tc Study

## Seed Material
- O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)

## Step A — Series Peak
Measured 18 compositions from unmeasurable.txt (despite file name, these had samples).

Results:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0 (also peak)
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

**T* = 127.0 K** (from O6.210Ca1Cu2Ba2Hg1)

## Step B — Filter Family B
threshold1 = 127.0 - 32 = 95.0

Materials with Hg and exactly 4 elements (O4.xxxCu1Ba2Hg1 series) with Tc >= 95.0:
- O4.047Cu1Ba2Hg1: 95.0 ✓
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
- O4.085Cu1Ba2Hg1: predicted ~97 ✓
- O4.101Cu1Ba2Hg1: predicted ~96 ✓
- O4.103Cu1Ba2Hg1: predicted ~96 ✓

O4.200Cu1Ba2Hg1: 94.2 < 95.0 (NOT in Q1)

## Step C — Extrapolate
**Rule:** Material is HIGH if it contains both Hg and Cu, otherwise LOW.

Predictions for unmeasured compositions:
- O4.101Cu1Ba2Hg1: HIGH (has Hg, Cu)
- O4.103Cu1Ba2Hg1: HIGH (has Hg, Cu)
- O4.085Cu1Ba2Hg1: HIGH (has Hg, Cu)
- O6Cu1Ba2Hg1Tl1: HIGH (has Hg, Cu)
- O10Ca2Cu3Ba2Tl2: LOW (no Hg)
- O6Cu1Cd0.250Ba2Tl1.750: LOW (no Hg)
- O7Cu3Ba2La1: LOW (no Hg)
- O7Cu3Y0.200Ba2La0.800: LOW (no Hg)

## Step D — Final Pick
Predicted HIGH unmeasured: O4.101, O4.103, O4.085, O6Cu1Ba2Hg1Tl1
- O4.101Cu1Ba2Hg1: 4 elements
- O4.103Cu1Ba2Hg1: 4 elements
- O4.085Cu1Ba2Hg1: 4 elements
- O6Cu1Ba2Hg1Tl1: 5 elements

Fewest elements: 4 (tie among O4.101, O4.103, O4.085)
Tie-break by higher expected Tc: O4.085 is closest to O4.081 (98.0), so expected ~97.

**Final answer: O4.085Cu1Ba2Hg1**
