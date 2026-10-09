# Notes - Superconductor Tc Study

## Seed Material
O6.080Ca1Cu2Ba2Hg1

## Step A - Homologous Series (same elements: O, Ca, Cu, Ba, Hg, varying oxygen)
Series members in measurable.txt (10 total):
1. O6.080Ca1Cu2Ba2Hg1 → 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 → 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 → 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 → 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 → 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 → 127.0 K ★ PEAK
7. O6.220Ca1Cu2Ba2Hg1 → 127.0 K ★ PEAK (tied)
8. O6.240Ca1Cu2Ba2Hg1 → 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 → 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 → 98.0 K

T* = 127.0 K (peak at O6.210Ca1Cu2Ba2Hg1, first occurrence)
threshold1 = 127.0 - 32 = 95.0 K

## Step B - 4-element Hg materials (measured 5 of 11)
- O4.047Cu1Ba2Hg1 → 95.0 K (>= 95.0 ✓)
- O4.096Cu1Ba2Hg1 → 95.6 K (>= 95.0 ✓)
- O4.150Cu1Ba2Hg1 → 97.0 K (>= 95.0 ✓)
- O4.200Cu1Ba2Hg1 → 94.2 K (< 95.0 ✗)
- O4.220Cu1Ba2Hg1 → 97.0 K (>= 95.0 ✓)

Unmeasured 4-elem Hg (predicted >= 95.0 by interpolation):
- O4.081Cu1Ba2Hg1 (O~4.081, between 4.047→95.0 and 4.096→95.6, ~95.3)
- O4.085Cu1Ba2Hg1 (O~4.085, ~95.4)
- O4.097Cu1Ba2Hg1 (O~4.097, very near 4.096→95.6, ~95.6)
- O4.101Cu1Ba2Hg1 (O~4.101, ~95.6)
- O4.103Cu1Ba2Hg1 (O~4.103, ~95.7)
- O4.162Cu1Ba2Hg1 (O~4.162, between 4.150→97.0 and 4.200→94.2, ~96.3)

Q1 = all measurable 4-elem Hg except O4.200 = 10 materials

## Other measurements (for rule induction)
- O7Cu3Ba2La1 → 89.6 K (LOW, 4 elements: O,Cu,Ba,La)
- O6Cu1Ba2Hg1Tl1 → 0.0 K (LOW, 5 elements: O,Cu,Ba,Hg,Tl)
- O10Ca2Cu3Ba2Tl2 → 118.97 K (HIGH, 5 elements: O,Ca,Cu,Ba,Tl)

## Step C - Rule Induction
All measured data:
HIGH (Tc >= 90):
- Series (10 items): all have Ca, Hg. Tc 92-127.
- 4-elem Hg (5 measured): O,Cu,Ba,Hg (no Ca, has Hg, no Tl). Tc 94.2-97.
- O10Ca2Cu3Ba2Tl2: has Ca, no Hg. Tc 118.97.

LOW (Tc < 90):
- O7Cu3Ba2La1: no Ca, no Hg, has La. Tc 89.6.
- O6Cu1Ba2Hg1Tl1: has Hg AND Tl, no Ca. Tc 0.0.

Rule: "Contains Ca OR (contains Hg AND does NOT contain Tl)"
- Has Ca → HIGH (series, O10Ca2Cu3Ba2Tl2)
- Has Hg without Tl → HIGH (4-element Hg)
- Has Hg with Tl and no Ca → LOW (O6Cu1Ba2Hg1Tl1)
- No Ca, no Hg → LOW (O7Cu3Ba2La1)

Verified on all 18 measurements ✓

## Step C - Predictions for unmeasurable
1. O6.160Ca1Cu2Ba2Hg1: has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Ca → HIGH
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → LOW
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → LOW

## Step D - Final Pick
HIGH unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Answer: O4.180Cu1Ba2Hg1 (4 elements, no tie to break)

## Budget tracking
Used: 18/18 (all spent)
