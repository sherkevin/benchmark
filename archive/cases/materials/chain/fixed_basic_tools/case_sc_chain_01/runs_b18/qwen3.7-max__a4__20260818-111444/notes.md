# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90 K, Low: < 90 K

## Step A: Homologous Series (same elements: O, Ca, Cu, Ba, Hg - 5 elements, varying oxygen)
Series members in measurable.txt (oxygen from 6.080 to 6.310):
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

## Step B: Hg-containing, exactly 4 elements (O, Cu, Ba, Hg)
11 materials: O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1

## Other measurable materials (for rule induction):
- O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl)
- O10Ca2Cu3Ba2Tl2 (5 elem: O,Ca,Cu,Ba,Tl)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elem: O,Cu,Cd,Ba,Tl)
- O7Cu3Ba2La1 (4 elem: O,Cu,Ba,La)
- O7Cu3Y0.200Ba2La0.800 (5 elem: O,Cu,Y,Ba,La)

## Unmeasurable materials (for prediction):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

## Measurement Log (18/18 used)

### Batch 1 (8 calls):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O7Cu3Ba2La1: 89.6 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

### Batch 2 (10 calls):
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK!
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.047Cu1Ba2Hg1: 95.0 K
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O4.097Cu1Ba2Hg1: 95.3 K

## Step A Results: Series Peak
Measured series (oxygen content vs Tc):
- O6.080: 92.0 K
- O6.140: 105.0 K
- O6.190: 122.0 K
- O6.220: 127.0 K ← MAXIMUM
- O6.240: 123.0 K
- O6.310: 98.0 K

**T* = 127.0 K** at composition **O6.220Ca1Cu2Ba2Hg1**

## Step B Results: Hg 4-element materials
threshold1 = T* - 32 = 127 - 32 = 95 K

Measured Hg 4-element materials (all >= 95 K):
- O4.081Cu1Ba2Hg1: 98.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K ✓
- O4.047Cu1Ba2Hg1: 95.0 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K ✓

Unmeasured (inferred >= 95 K based on narrow range 95-98 K):
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1

**Q1 = all 11 Hg 4-element materials**

## Step C: Rule Induction

### Measured materials analysis:
HIGH (Tc >= 90 K):
- All O6.xxxCa1Cu2Ba2Hg1 (5 elem, has Ca, has Hg): 92-127 K
- O10Ca2Cu3Ba2Tl2 (5 elem, has Ca, no Hg): 118.97 K
- All O4.xxxCu1Ba2Hg1 (4 elem, no Ca, has Hg, O≈4): 95-98 K

LOW (Tc < 90 K):
- O7Cu3Ba2La1 (4 elem, no Ca, no Hg): 89.6 K
- O7Cu3Y0.200Ba2La0.800 (5 elem, no Ca, no Hg): 81.0 K
- O6Cu1Ba2Hg1Tl1 (5 elem, no Ca, has Hg, O=6): 0.0 K ← KEY!
- O6Cu1Cd0.250Ba2Tl1.750 (5 elem, no Ca, no Hg): 88.0 K

### Induced Rule:
**HIGH if (contains Ca) OR (contains Hg AND oxygen < 5)**

This explains:
- Ca-containing materials are always HIGH
- Hg-containing materials with low oxygen (~4) are HIGH
- Hg-containing materials with high oxygen (6+) are LOW (O6Cu1Ba2Hg1Tl1 = 0 K)
- Materials without Ca or Hg are LOW

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca → **HIGH**
3. O4.180Cu1Ba2Hg1: no Ca, has Hg, O=4.180 < 5 → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH**
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → **LOW**
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → **LOW**

## Step D: Final Pick
Among HIGH predictions, find fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected HIGH based on rule)
