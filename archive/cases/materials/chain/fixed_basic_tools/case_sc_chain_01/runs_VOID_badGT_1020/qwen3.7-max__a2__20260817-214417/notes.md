# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- High Tc: >= 90 K, Low: < 90 K

## Step A - Homologous Series (same elements as seed, varying O)
12 members to measure

## Step B - Hg + exactly 4 elements
12 members (O4.xxxCu1Ba2Hg1 series)

## Other measurable (5 materials)
Lines 25-29

## Measurements - All Results

### Homologous Series (Step A):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (HIGH) *** PEAK ***
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (HIGH) *** PEAK TIE ***
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (HIGH)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (HIGH)

T* = 127.0 K, Peak = O6.210Ca1Cu2Ba2Hg1

### 4-element Hg (Step B) - threshold1 = 95.0:
- O4.081Cu1Ba2Hg1: 98.0 K ✓ Q1
- O4.162Cu1Ba2Hg1: 97.4 K ✓ Q1
- O4.150Cu1Ba2Hg1: 97.0 K ✓ Q1
- O4.220Cu1Ba2Hg1: 97.0 K ✓ Q1
- O4.096Cu1Ba2Hg1: 95.6 K ✓ Q1
- O4.097Cu1Ba2Hg1: 95.3 K ✓ Q1
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O4.180Cu1Ba2Hg1: NO SAMPLE
- O4.047Cu1Ba2Hg1: NO SAMPLE

### Other measurable:
- O6Cu1Ba2Hg1Tl1: 0.0 K (LOW) - {O,Cu,Ba,Hg,Tl}
- O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH) - {O,Ca,Cu,Ba,Tl}
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (LOW) - {O,Cu,Cd,Ba,Tl}
- O7Cu3Ba2La1: 89.6 K (LOW) - {O,Cu,Ba,La}
- O7Cu3Y0.200Ba2La0.800: 81.0 K (LOW) - {O,Cu,Y,Ba,La}

### Unmeasurable (all confirmed no sample):
O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1,
O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800,
O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130

## Rule Induction (Step C)
HIGH materials share: contain Hg without Tl, OR contain Ca
LOW materials: lack both conditions

Rule: HIGH iff (contains Hg AND NOT contains Tl) OR (contains Ca)

Verified on all 25 measured materials:
- All O6.xxxCa1Cu2Ba2Hg1: Hg+Ca, no Tl → HIGH ✓
- All O4.xxxCu1Ba2Hg1: Hg, no Tl → HIGH ✓
- O10Ca2Cu3Ba2Tl2: has Ca → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Hg+Tl, no Ca → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, no Ca → LOW ✓
- O7Cu3Ba2La1: no Hg, no Ca → LOW ✓
- O7Cu3Y0.200Ba2La0.800: no Hg, no Ca → LOW ✓

## Predictions:
1. O6.160Ca1Cu2Ba2Hg1: Hg+Ca, no Tl → HIGH (5 elem)
2. O6.270Ca1Cu2Ba2Hg1: Hg+Ca, no Tl → HIGH (5 elem)
3. O4.180Cu1Ba2Hg1: Hg, no Tl → HIGH (4 elem)
4. O4.047Cu1Ba2Hg1: Hg, no Tl → HIGH (4 elem)
5. O4.066Cu1Ba2Hg1: Hg, no Tl → HIGH (4 elem)
6. O4.160Cu1Ba2Hg1: Hg, no Tl → HIGH (4 elem)
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH (5 elem)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH (6 elem)
9. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → LOW (5 elem)
10. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → LOW (5 elem)

## Step D:
HIGH with fewest elements (4): O4.180, O4.047, O4.066, O4.160
Expected Tc by proximity to measured:
- O4.160: ~97 (near O4.162=97.4, O4.150=97.0) ← HIGHEST
- O4.180: ~96 (between O4.162=97.4, O4.200=94.2)
- O4.066: ~95 (between O4.081=98.0, O4.085=92.0)
- O4.047: uncertain (extrapolation)

Final pick: O4.160Cu1Ba2Hg1
