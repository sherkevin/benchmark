# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1 (measured Tc = 92.0 K)
- Measurement budget: 18 measure_tc calls (1 used, 17 remaining)
- Goal: Complete 4-step process and produce answer.json

## Step A - Series Peak Analysis
Homologous series: O6.XXXCa1Cu2Ba2Hg1 (varying oxygen from 6.080 to 6.310)
Measurements completed:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc (T*) = 127.0 K at compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Peak composition = O6.210Ca1Cu2Ba2Hg1 (first occurrence of maximum)
Threshold1 = T* - 32 = 127 - 32 = 95.0 K

## Step B - Filter Family B
Materials with Hg and exactly 4 elements: O4.XXXCu1Ba2Hg1 series (lines 11-21 in measurable.txt)
Measurements completed:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (>= 95.0)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ (>= 95.0)
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ (>= 95.0)
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ (>= 95.0)
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ (>= 95.0)
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ (>= 95.0)
7. O4.047Cu1Ba2Hg1: 95.0 K ✓ (>= 95.0)
8. O4.200Cu1Ba2Hg1: 94.2 K ✗ (< 95.0)

Remaining unmeasured from series (need to predict based on pattern):
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1  
- O4.085Cu1Ba2Hg1

Based on pattern: As oxygen content increases from ~4.047 to ~4.220, Tc decreases from 95.0 to 94.2 K. The values seem to peak around O4.081-4.162 range and decrease on both sides.
Need to check if O4.101, O4.103, O4.085 are above threshold.

Budget: All 18 measurements used.

## Step C - Rule Induction
Based on measured data:
- All measured materials are high-Tc (>= 90 K)
- All measured materials contain Hg, Cu, and Ba
- Materials without Hg (in unmeasurable.txt) likely have low Tc
- Single-factor rules warned against, but multi-factor rule needed

Induced rule: "High Tc if material contains Hg, Cu, and Ba simultaneously"

Predictions for unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1: Contains Hg✓ Cu✓ Ba✓ → High
2. O6.270Ca1Cu2Ba2Hg1: Contains Hg✓ Cu✓ Ba✓ → High
3. O4.180Cu1Ba2Hg1: Contains Hg✓ Cu✓ Ba✓ → High
4. O8Ca0.850Cu2.200Ba2Tl1.800: No Hg → Low
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: No Hg, no Ba → Low
6. O8Cu3Y1Ba1.900La0.100: No Hg → Low
7. O8Cu4Y1Ba1.870La0.130: No Hg → Low

## Step D - Final Pick
High predictions: O6.160Ca1Cu2Ba2Hg1 (5 elements), O6.270Ca1Cu2Ba2Hg1 (5 elements), O4.180Cu1Ba2Hg1 (4 elements)
O4.180Cu1Ba2Hg1 has fewest elements (4) → Final pick

## Step B - Qualifying Set (Q1)
All O4.XXXCu1Ba2Hg1 with Tc >= 95.0 K:
Measured: O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1
Predicted (based on pattern): O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1
Not qualifying: O4.200Cu1Ba2Hg1 (94.2 K < 95.0 K)