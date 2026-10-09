# Notes: Superconductor Tc Study

## Seed: O6.080Ca1Cu2Ba2Hg1

## Step A — Series Peak (measurements complete)

### O6.xxxCa1Cu2Ba2Hg1 series (5 elements: O, Ca, Cu, Ba, Hg):
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 ★ PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 ★ PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

### O4.xxxCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg):
| Composition | Tc (K) |
|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 |
| O4.162Cu1Ba2Hg1 | 97.4 |
| O4.150Cu1Ba2Hg1 | 97.0 |
| O4.220Cu1Ba2Hg1 | 97.0 |
| O4.096Cu1Ba2Hg1 | 95.6 |
| O4.097Cu1Ba2Hg1 | 95.3 |
| O4.047Cu1Ba2Hg1 | 95.0 |
| O4.200Cu1Ba2Hg1 | 94.2 |
| O4.101Cu1Ba2Hg1 | 93.4 |
| O4.103Cu1Ba2Hg1 | 92.5 |
| O4.085Cu1Ba2Hg1 | 92.0 |

### Other measurable:
| Composition | Tc (K) | Elements |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | O, Cu, Ba, Hg, Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O, Ca, Cu, Ba, Tl (5) |
| O7Cu3Ba2La1 | 89.6 | O, Cu, Ba, La (4) |
| O6Cu1Cd0.250Ba2Tl1.750 | NOT MEASURED (budget) | O, Cu, Cd, Ba, Tl (5) |
| O7Cu3Y0.200Ba2La0.800 | NOT MEASURED (budget) | O, Cu, Y, Ba, La (5) |

**T* = 127.0 K** (O6.210Ca1Cu2Ba2Hg1 — first occurrence of peak)

## Step B — Filter Family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials with Hg and exactly 4 elements (from measurable, with Tc >= 95.0):
- O4.081Cu1Ba2Hg1: 98.0 ≥ 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ≥ 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 ≥ 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 ≥ 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ≥ 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 ≥ 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 ≥ 95.0 ✓

Excluded (below threshold):
- O4.200Cu1Ba2Hg1: 94.2 < 95.0
- O4.101Cu1Ba2Hg1: 93.4 < 95.0
- O4.103Cu1Ba2Hg1: 92.5 < 95.0
- O4.085Cu1Ba2Hg1: 92.0 < 95.0

**Q1 = 7 members**

## Step C — Rule Induction

### Training data summary:

**HIGH (Tc >= 90 K) — 22 materials:**
- 10× O6.xxxCa1Cu2Ba2Hg1 (5 elem, has Ca, has Hg, no Tl)
- 11× O4.xxxCu1Ba2Hg1 (4 elem, no Ca, has Hg, no Tl)
- 1× O10Ca2Cu3Ba2Tl2 (5 elem, has Ca, no Hg, has Tl)

**LOW (Tc < 90 K) — 2 materials:**
- 1× O6Cu1Ba2Hg1Tl1 (5 elem, no Ca, has Hg, has Tl) — Tc = 0.0
- 1× O7Cu3Ba2La1 (4 elem, no Ca, no Hg, no Tl) — Tc = 89.6

### Induced Rule:
A material is **high** if:
- It contains **Ca** (regardless of other elements), OR
- It contains **Hg** AND does NOT contain **Tl**

A material is **low** if:
- It does NOT contain Ca, AND
- Either it does NOT contain Hg, OR it contains both Hg and Tl

This is an interaction rule involving Ca, Hg, and Tl presence.

### Verification against training data:
- O6.xxxCa1Cu2Ba2Hg1: has Ca → HIGH ✓
- O4.xxxCu1Ba2Hg1: no Ca, has Hg, no Tl → HIGH ✓
- O10Ca2Cu3Ba2Tl2: has Ca → HIGH ✓
- O6Cu1Ba2Hg1Tl1: no Ca, has Hg AND Tl → LOW ✓
- O7Cu3Ba2La1: no Ca, no Hg → LOW ✓

All 24 measured materials correctly classified!

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca → **HIGH**
3. O4.180Cu1Ba2Hg1: no Ca, has Hg, no Tl → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH**
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → **LOW**
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → **LOW**

## Step D — Final Pick
Among predicted-HIGH unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: **4 elements** ← fewest!
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among predicted-high)
