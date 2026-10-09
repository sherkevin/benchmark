# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls (used 25)
- Definition: HIGH = Tc >= 90 K, LOW = Tc < 90 K

## Step A — Homologous Series Peak
Series: O6.xxxCa1Cu2Ba2Hg1 (elements: O, Ca, Cu, Ba, Hg)

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K**, peak composition: **O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter Family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = **95.0 K**

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | — |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | — |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |

**Q1** = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}

## Step C — Rule Induction

### All measured materials with features:

**HIGH (Tc >= 90):**
- O6.xxxCa1Cu2Ba2Hg1 (10 measured): has Ca, has Hg, 5 elements
- O4.xxxCu1Ba2Hg1 (10 measured): no Ca, has Hg, 4 elements
- O10Ca2Cu3Ba2Tl2 (118.97 K): has Ca, no Hg, 5 elements

**LOW (Tc < 90):**
- O6Cu1Ba2Hg1Tl1 (0.0 K): no Ca, has Hg, 5 elements
- O6Cu1Cd0.250Ba2Tl1.750 (88.0 K): no Ca, no Hg, 5 elements
- O7Cu3Ba2La1 (89.6 K): no Ca, no Hg, 4 elements
- O7Cu3Y0.200Ba2La0.800 (81.0 K): no Ca, no Hg, 5 elements

### Induced Rule:
**HIGH if: contains Ca OR (contains Hg AND has exactly 4 elements)**

This is an interaction rule — neither "contains Ca" alone nor "contains Hg" alone nor "has 4 elements" alone correctly classifies all materials.

Verification:
- O6Cu1Ba2Hg1Tl1 (LOW): no Ca, has Hg but 5 elements → neither condition → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750 (LOW): no Ca, no Hg → LOW ✓
- O7Cu3Ba2La1 (LOW): no Ca, no Hg → LOW ✓
- O7Cu3Y0.200Ba2La0.800 (LOW): no Ca, no Hg → LOW ✓
- All Ca-containing → HIGH ✓
- All Hg+4element → HIGH ✓

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca → **HIGH**
3. O4.180Cu1Ba2Hg1: Hg + 4 elem → **HIGH**
4. O4.047Cu1Ba2Hg1: Hg + 4 elem → **HIGH**
5. O4.066Cu1Ba2Hg1: Hg + 4 elem → **HIGH**
6. O4.160Cu1Ba2Hg1: Hg + 4 elem → **HIGH**
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH**
9. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → **LOW**
10. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → **LOW**

## Step D — Final Pick
Among predicted HIGH, sorted by element count:
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Four-way tie at 4 elements. Tie-break: higher expected Tc.
From O4.xxxCu1Ba2Hg1 measurements, O4.162 has Tc=97.4 (highest measured after O4.081=98.0).
O4.160 is closest to O4.162 → expected ~97 K.
O4.180 is between O4.162(97.4) and O4.200(94.2) → expected ~95 K.
O4.066 and O4.047 are in unmeasured region below O4.081.

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc among tied candidates)
