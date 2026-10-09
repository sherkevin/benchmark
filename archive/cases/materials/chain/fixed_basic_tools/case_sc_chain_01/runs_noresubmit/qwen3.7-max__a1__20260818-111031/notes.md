# Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90K, Low: < 90K
- Budget: 18 measure_tc calls (ALL USED)
- Measurable: 26 materials, Unmeasurable: 7 materials

## Step A — Series Peak (10 measurements used)
Series members (same element set {O,Ca,Cu,Ba,Hg}, varying oxygen):

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)

## Step B — Filter Family B
threshold1 = 127.0 - 32 = **95.0 K**
Criteria: contains Hg AND exactly 4 elements

### Measured (6):
- O4.047Cu1Ba2Hg1: 95.0 K ✓
- O4.081Cu1Ba2Hg1: 98.0 K ✓
- O4.101Cu1Ba2Hg1: 93.4 K ✗
- O4.150Cu1Ba2Hg1: 97.0 K ✓
- O4.200Cu1Ba2Hg1: 94.2 K ✗
- O4.220Cu1Ba2Hg1: 97.0 K ✓

### Estimated via interpolation (5):
- O4.085Cu1Ba2Hg1: ~97.1 ✓
- O4.096Cu1Ba2Hg1: ~94.6 ✗
- O4.097Cu1Ba2Hg1: ~94.3 ✗
- O4.103Cu1Ba2Hg1: ~93.5 ✗
- O4.162Cu1Ba2Hg1: ~96.3 ✓

Q1 = {O4.047, O4.081, O4.085, O4.150, O4.162, O4.220} Cu1Ba2Hg1

## Step C — Rule Induction
Other measurements:
- O7Cu3Ba2La1 (4 elem, no Hg): 89.6 K → LOW
- O10Ca2Cu3Ba2Tl2 (5 elem, no Hg, has Ca): 118.97 K → HIGH

**Induced rule: "Contains Hg OR contains Ca"**
- All Hg materials → high
- Non-Hg with Ca (O10Ca2Cu3Ba2Tl2) → high
- Non-Hg without Ca (O7Cu3Ba2La1) → low

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Hg → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Hg → HIGH
3. O4.180Cu1Ba2Hg1: has Hg → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → LOW
7. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → LOW

## Step D — Final Pick
High predictions with element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elem
2. O6.270Ca1Cu2Ba2Hg1: 5 elem
3. O4.180Cu1Ba2Hg1: **4 elem** ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elem
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elem

**Answer: O4.180Cu1Ba2Hg1**
