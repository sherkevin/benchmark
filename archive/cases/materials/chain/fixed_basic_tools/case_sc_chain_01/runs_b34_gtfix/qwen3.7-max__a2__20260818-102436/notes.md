# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- High Tc: >= 90 K, Low Tc: < 90 K

## Step A — Series Peak
Measured all 26 measurable materials (used 26 of 34 budget).

### Results:
**O6.xxxCa1Cu2Ba2Hg1 series (5 elements: O, Ca, Cu, Ba, Hg):**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) ← MAX
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) ← MAX (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

**O4.xxxCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg):**
- O4.081Cu1Ba2Hg1: 98.0 K (high)
- O4.162Cu1Ba2Hg1: 97.4 K (high)
- O4.150Cu1Ba2Hg1: 97.0 K (high)
- O4.220Cu1Ba2Hg1: 97.0 K (high)
- O4.096Cu1Ba2Hg1: 95.6 K (high)
- O4.097Cu1Ba2Hg1: 95.3 K (high)
- O4.047Cu1Ba2Hg1: 95.0 K (high)
- O4.200Cu1Ba2Hg1: 94.2 K (high)
- O4.101Cu1Ba2Hg1: 93.4 K (high)
- O4.103Cu1Ba2Hg1: 92.5 K (high)
- O4.085Cu1Ba2Hg1: 92.0 K (high)

**Other materials:**
- O6Cu1Ba2Hg1Tl1: 0.0 K (low) - 5 elements
- O10Ca2Cu3Ba2Tl2: 118.97 K (high) - 5 elements
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low) - 5 elements
- O7Cu3Ba2La1: 89.6 K (low) - 4 elements
- O7Cu3Y0.200Ba2La0.800: 81.0 K (low) - 5 elements

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg with exactly 4 elements and Tc >= 95.0 K:
- O4.081Cu1Ba2Hg1: 98.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K ✓
- O4.047Cu1Ba2Hg1: 95.0 K ✓

**Q1 = 7 materials** (all from O4.xxxCu1Ba2Hg1 series with Tc >= 95.0)

## Step C — Induced Rule
Analyzing patterns in measurable materials:

**High Tc materials have:**
- Ca present (O6.xxxCa1Cu2Ba2Hg1, O10Ca2Cu3Ba2Tl2)
- OR Hg present WITHOUT Tl (O4.xxxCu1Ba2Hg1)

**Low Tc materials:**
- O6Cu1Ba2Hg1Tl1: has Hg AND Tl (no Ca) → 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, no Hg → 88.0 K
- O7Cu3Ba2La1: no Ca, no Hg → 89.6 K
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg → 81.0 K

**Rule: High Tc if (contains Ca) OR (contains Hg AND does NOT contain Tl)**

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca → **HIGH**
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH**
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → **LOW**
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → **LOW**

## Step D — Final Pick
Among predicted HIGH, find fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: **4 elements** ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected high Tc based on Hg presence without Tl)
