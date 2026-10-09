# Notes on Superconductor Tc Study

## Step A — Series Peak (DONE)

Measured all series members (O6.xxxCa1Cu2Ba2Hg1):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAX)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

## Step B — Filter Family B (DONE)

Measured O-Cu-Ba-Hg (4 elements, contains Hg):
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗

**Q1** = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]

Also measured other materials:
- O6Cu1Ba2Hg1Tl1: 0.0 K (LOW)
- O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH)
- O7Cu3Ba2La1: 89.6 K (LOW)

## Step C — Induced Rule (DONE)

Pattern from measurements:
- Has Ca → HIGH (all Ca-containing materials >= 90 K)
- No Ca, has Hg, no Tl → HIGH (O-Cu-Ba-Hg all >= 90 K)
- No Ca, has Hg, has Tl → LOW (O6Cu1Ba2Hg1Tl1 = 0 K)
- No Ca, no Hg → LOW (O7Cu3Ba2La1 = 89.6 K)

**Rule: HIGH if (contains Ca) OR (contains Hg AND NOT contains Tl)**

Predictions for unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: HIGH (has Ca)
- O6.270Ca1Cu2Ba2Hg1: HIGH (has Ca)
- O4.180Cu1Ba2Hg1: HIGH (has Hg, no Tl)
- O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (has Ca)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (has Ca)
- O8Cu3Y1Ba1.900La0.100: LOW (no Ca, no Hg)
- O8Cu4Y1Ba1.870La0.130: LOW (no Ca, no Hg)

## Step D — Final Pick (DONE)

HIGH predictions with element counts:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

**Final answer: O4.180Cu1Ba2Hg1**
