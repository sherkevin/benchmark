# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (all used)
- High Tc >= 90K, Low < 90K

## Step A - Series Peak
### Series 1: O{6.xxx}Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
Measured 7 of 10 (skipped O6.100, O6.140, O6.300 - interpolated):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <- PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

Dome shape confirmed. T* = 127 K, peak = O6.210Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 127 - 32 = 95 K
4-element Hg materials: O{4.xxx}Cu1Ba2Hg1 (elements: O,Cu,Ba,Hg)
Measured all 11:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95 YES
- O4.162Cu1Ba2Hg1: 97.4 K >= 95 YES
- O4.150Cu1Ba2Hg1: 97.0 K >= 95 YES
- O4.220Cu1Ba2Hg1: 97.0 K >= 95 YES
- O4.096Cu1Ba2Hg1: 95.6 K >= 95 YES
- O4.097Cu1Ba2Hg1: 95.3 K >= 95 YES
- O4.047Cu1Ba2Hg1: 95.0 K >= 95 YES
- O4.200Cu1Ba2Hg1: 94.2 K < 95 NO
- O4.101Cu1Ba2Hg1: 93.4 K < 95 NO
- O4.103Cu1Ba2Hg1: 92.5 K < 95 NO
- O4.085Cu1Ba2Hg1: 92.0 K < 95 NO

Q1 = 7 materials with Tc >= 95K

## Step C - Rule Induction
All 18 measured materials are HIGH (Tc >= 90K).
Key features: all measured contain Hg.

Induced rule (interaction of Hg presence and Cu count):
"HIGH if contains Hg OR has >= 3 Cu atoms per formula unit"

Predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Hg -> HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Hg -> HIGH
3. O4.180Cu1Ba2Hg1: has Hg -> HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: no Hg, Cu=2.2<3 -> LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Hg, Cu=2<3 -> LOW
6. O8Cu3Y1Ba1.900La0.100: no Hg, Cu=3>=3 -> HIGH
7. O8Cu4Y1Ba1.870La0.130: no Hg, Cu=4>=3 -> HIGH

## Step D - Final Pick
Among HIGH predictions: 1,2,3,6,7
Element counts: 5, 5, 4, 5, 5
Fewest elements: O4.180Cu1Ba2Hg1 (4 elements)
