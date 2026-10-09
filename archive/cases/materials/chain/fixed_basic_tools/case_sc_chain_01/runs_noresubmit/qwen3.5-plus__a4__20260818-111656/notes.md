# Notes on Superconductor Tc Study

## Step A - Find peak Tc in the homologous series

Seed: O6.080Ca1Cu2Ba2Hg1

Measured series members (O6.xCa1Cu2Ba2Hg1):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← also peak
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B - Filter family B (Hg, exactly 4 elements, Tc >= threshold1)

threshold1 = T* - 32 = 127.0 - 32 = 95.0

Measured O4.xCu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg):
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.047Cu1Ba2Hg1: 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 ✗ (below threshold)
- O4.101Cu1Ba2Hg1: not measured (budget exhausted)
- O4.103Cu1Ba2Hg1: not measured
- O4.085Cu1Ba2Hg1: not measured

**Q1** = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.047Cu1Ba2Hg1"]

## Step C - Extrapolate rule for unmeasurable materials

Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

**Induced rule:** A material is HIGH if it contains (Hg OR Y) AND Cu AND Ba.

This is a multi-factor rule requiring:
- Either Hg or Y present
- AND Cu present
- AND Ba present

Predictions:
- O6.160Ca1Cu2Ba2Hg1: has Hg, Cu, Ba → HIGH
- O6.270Ca1Cu2Ba2Hg1: has Hg, Cu, Ba → HIGH
- O4.180Cu1Ba2Hg1: has Hg, Cu, Ba → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: has Cu, Ba, but no Hg or Y → LOW
- O7Ca1Cu2Sr2Tl0.500Pb0.500: has Cu, no Ba (has Sr), no Hg or Y → LOW
- O8Cu3Y1Ba1.900La0.100: has Y, Cu, Ba → HIGH
- O8Cu4Y1Ba1.870La0.130: has Y, Cu, Ba → HIGH

## Step D - Final pick

Among HIGH predictions, find the one with FEWEST elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
- O8Cu3Y1Ba1.900La0.100: 5 elements (O, Cu, Y, Ba, La)
- O8Cu4Y1Ba1.870La0.130: 5 elements (O, Cu, Y, Ba, La)

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among HIGH predictions)
