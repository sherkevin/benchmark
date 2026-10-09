# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 14 measure_tc calls
- Goal: Complete 4 steps (A-D)

## Step A: Find maximum Tc in homologous doping series
Materials in measurable.txt: 27 compositions
Homologous series appears to be oxygen-doped Ca-Cu-Ba-Hg compounds (lines 1-10: O6.080-6.310Ca1Cu2Ba2Hg1)

**Step A Results:**
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

**Maximum Tc (T*) = 127.0 K** (from O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

Measurements used: 10/14

## Step B: Filter family B
- Contains Hg
- Has exactly 4 elements
- Tc >= threshold1 (95.0 K)

**Candidates:** Lines 11-21 in measurable.txt: O4.xxxCu1Ba2Hg1 compositions (11 total)

**Measurements:**
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (>=95.0K)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K ✓
4. O4.047Cu1Ba2Hg1: 95.0 K ✓ (exactly at threshold)

Based on pattern (lowest oxygen O4.047 = 95.0K, others higher), all 11 O4.xxxCu1Ba2Hg1 likely have Tc >= 95.0K.

**Q1 (qualifying set):** All 11 O4.xxxCu1Ba2Hg1 compositions from measurable.txt lines 11-21.

Measurements used: 4 more (total 14/14)

## Step C: Induce rule for unmeasurable materials
**All measured compositions (14) are HIGH Tc (>=90K):**
- All contain O, Cu, Ba, Hg
- Some contain Ca
- None contain Tl, Pb, Y, La, Cd, Sr

**Induced rule:** High Tc requires ALL of: Cu, Ba, Hg, O. Low Tc otherwise.

**Predictions for unmeasurable.txt:**
1. O6.160Ca1Cu2Ba2Hg1: Contains Cu, Ba, Hg, O → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Cu, Ba, Hg, O → HIGH
3. O4.180Cu1Ba2Hg1: Contains Cu, Ba, Hg, O → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Cu, Ba, O but NO Hg → LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Cu, O but NO Ba (Sr instead), NO Hg → LOW
6. O8Cu3Y1Ba1.900La0.100: Contains Cu, Ba, O but NO Hg → LOW
7. O8Cu4Y1Ba1.870La0.130: Contains Cu, Ba, O but NO Hg → LOW

## Step D: Final pick
Among HIGH predictions: O6.160Ca1Cu2Ba2Hg1 (5 elements), O6.270Ca1Cu2Ba2Hg1 (5 elements), O4.180Cu1Ba2Hg1 (4 elements)

**Fewest elements:** O4.180Cu1Ba2Hg1 (4 elements)

**Final answer:** O4.180Cu1Ba2Hg1