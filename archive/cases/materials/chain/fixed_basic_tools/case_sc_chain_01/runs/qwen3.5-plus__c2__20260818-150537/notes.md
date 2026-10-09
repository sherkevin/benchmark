# Notes for Superconductor Tc Study

## Step A - Finding peak Tc in the homologous series
The seed is O6.080Ca1Cu2Ba2Hg1. The series has same element set, varying oxygen.
Need to measure all members in measurable.txt to find max Tc.

## Step B - Filter family B
threshold1 = T* - 32
Find materials with Hg and exactly 4 elements with Tc >= threshold1

## Step C - Extrapolate for unmeasurable
Induce a rule to predict high/low for unmeasurable materials

## Step D - Final pick
Among predicted high unmeasurable, find one with fewest elements (tie-break: higher expected Tc)

---

## Measurements so far:

### Step A - Series with same elements as seed (O, Ca, Cu, Ba, Hg):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

### Step B - Materials with Hg and exactly 4 elements (O, Cu, Ba, Hg):
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 K >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0 ✗
- O4.101Cu1Ba2Hg1: not measured (budget exhausted)
- O4.103Cu1Ba2Hg1: not measured
- O4.085Cu1Ba2Hg1: not measured

**Q1** = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.047Cu1Ba2Hg1"]

Budget used: 18/18

### Step C - Induced Rule
Rule: HIGH if (contains Hg) OR (has exactly 5 elements)

Predictions for unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: has Hg → HIGH
- O6.270Ca1Cu2Ba2Hg1: has Hg → HIGH
- O4.180Cu1Ba2Hg1: has Hg → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements, no Hg → HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements, no Hg → LOW
- O8Cu3Y1Ba1.900La0.100: 5 elements, no Hg → HIGH
- O8Cu4Y1Ba1.870La0.130: 5 elements, no Hg → HIGH

### Step D - Final Pick
Among HIGH predictions, find fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O8Cu3Y1Ba1.900La0.100: 5 elements
- O8Cu4Y1Ba1.870La0.130: 5 elements

**Final answer: O4.180Cu1Ba2Hg1**

