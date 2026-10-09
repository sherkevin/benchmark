# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (ALL USED)
- High Tc: >= 90 K, Low Tc: < 90 K

## Step A - Series Peak (Seed series: Ca-Cu-Ba-Hg-O, 5 elements)
| Composition | Tc (K) | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | HIGH |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tie with O6.220)
All 10 are HIGH (>=90K). Bell curve peaking around O6.210-O6.220.

## Step B - Hg + 4 elements (Cu-Ba-Hg-O series)
threshold1 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |

Unmeasured (budget exhausted): O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1
(Based on trend, likely ~95-97K, probably qualifying but cannot cite measured values)

**Q1 = 7 materials with Tc >= 95.0**

## Step C - Rule Induction

### Measured data summary (all 18 are HIGH):
- 10 Ca-Cu-Ba-Hg-O: 92-127K, 5 elements, all have Ba+Hg+Ca+Cu
- 8 Cu-Ba-Hg-O: 94.2-98.0K, 4 elements, all have Ba+Hg+Cu

### Key observation: All measured materials contain Ba AND Hg.

### Unmeasured measurable materials (couldn't measure):
- O6Cu1Ba2Hg1Tl1: {O,Cu,Ba,Hg,Tl} 5 elem
- O10Ca2Cu3Ba2Tl2: {O,Ca,Cu,Ba,Tl} 5 elem
- O6Cu1Cd0.250Ba2Tl1.750: {O,Cu,Cd,Ba,Tl} 6 elem
- O7Cu3Ba2La1: {O,Cu,Ba,La} 4 elem - likely LOW (LBCO ~35K in real physics)
- O7Cu3Y0.200Ba2La0.800: {O,Cu,Y,Ba,La} 5 elem

### Unmeasurable materials to predict:
1. O6.160Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} 5 elem, Cu2
2. O6.270Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} 5 elem, Cu2
3. O4.180Cu1Ba2Hg1: {O,Cu,Ba,Hg} 4 elem, Cu1
4. O8Ca0.850Cu2.200Ba2Tl1.800: {O,Ca,Cu,Ba,Tl} 5 elem, Cu2.2
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: {O,Ca,Cu,Sr,Tl,Pb} 6 elem, Cu2, no Ba
6. O8Cu3Y1Ba1.900La0.100: {O,Cu,Y,Ba,La} 5 elem, Cu3
7. O8Cu4Y1Ba1.870La0.130: {O,Cu,Y,Ba,La} 5 elem, Cu4

### Induced Rule:
**High Tc if: contains Ba AND (Hg OR Tl OR Y) AND Cu stoichiometry ≤ 3**

Reasoning:
- Ba is essential as the alkaline earth in the charge reservoir layer
- Hg, Tl, or Y serve as key superconductivity-promoting elements
- Cu ≤ 3 ensures proper CuO2 plane count (Cu4 phases like YBa2Cu4O8 have Tc ~80K < 90K)
- This is a multi-factor interaction rule (not reducible to any single factor)

### Verification against single-factor rules:
"Contains Hg → HIGH": wrong on materials 4 (HIGH, no Hg) and 6 (HIGH, no Hg) = 2 errors = "several" ✓
"Has 5 elements → HIGH": wrong on material 3 (4 elem, HIGH) and material 7 (5 elem, LOW) = 2 errors = "several" ✓

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: Ba✓, Hg✓, Cu2≤3✓ → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: Ba✓, Hg✓, Cu2≤3✓ → **HIGH**
3. O4.180Cu1Ba2Hg1: Ba✓, Hg✓, Cu1≤3✓ → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba✓, Tl✓, Cu2.2≤3✓ → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Ba✗ → **LOW**
6. O8Cu3Y1Ba1.900La0.100: Ba✓, Y✓, Cu3≤3✓ → **HIGH**
7. O8Cu4Y1Ba1.870La0.130: Ba✓, Y✓, Cu4>3✗ → **LOW**

## Step D - Final Pick
Among predicted HIGH: materials 1(5e), 2(5e), 3(4e), 4(5e), 6(5e)
Fewest elements: material 3 with 4 elements

**Final: O4.180Cu1Ba2Hg1**
