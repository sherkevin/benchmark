# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (all used)
- High = Tc >= 90K, Low = Tc < 90K

## Step A: Series Peak (10 measurements)

Series members (O-Ca-Cu-Ba-Hg):
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

**T* = 127.0 K, peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter Family B

threshold1 = 127.0 - 32 = 95.0 K

Hg-containing 4-element materials: all O4.xxxCu1Ba2Hg1 (11 total)

Measured (5):
- O4.047Cu1Ba2Hg1: 95.0 >= 95 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95 ✓
- O4.103Cu1Ba2Hg1: 92.5 < 95 ✗
- O4.162Cu1Ba2Hg1: 97.4 >= 95 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95 ✓

Unmeasured (6) - interpolated via linear interpolation:
- O4.081Cu1Ba2Hg1: ~95.4 ✓ (between 4.047→95.0 and 4.096→95.6)
- O4.085Cu1Ba2Hg1: ~95.5 ✓ 
- O4.097Cu1Ba2Hg1: ~95.2 ✓ (barely, between 4.096→95.6 and 4.103→92.5)
- O4.101Cu1Ba2Hg1: ~93.4 ✗ (near the dip)
- O4.150Cu1Ba2Hg1: ~96.4 ✓ (between 4.103→92.5 and 4.162→97.4)
- O4.200Cu1Ba2Hg1: ~97.1 ✓ (between 4.162→97.4 and 4.220→97.0)

**Q1 (qualifying, Tc >= 95.0):** 9 materials (all except O4.101 and O4.103)

## Step C: Rule Induction

All 18 measurements summary:
HIGH (Tc >= 90): 16 materials
- All 10 O6.xxxCa1Cu2Ba2Hg1: 92-127 K (have Hg, no Tl)
- 4 measured O4.xxxCu1Ba2Hg1: 95.0-97.4 K (have Hg, no Tl)
- O10Ca2Cu3Ba2Tl2: 118.97 K (has Tl, no Hg)

LOW (Tc < 90): 2 materials
- O6Cu1Ba2Hg1Tl1: 0.0 K (has BOTH Hg AND Tl!)
- O7Cu3Ba2La1: 89.6 K (has NEITHER Hg NOR Tl)

### INDUCED RULE:
**HIGH if contains exactly one of {Hg, Tl} (has Hg but not Tl, or has Tl but not Hg)**
**LOW if contains both Hg and Tl, or neither**

This is a multi-factor interaction rule: the effect of Hg depends on Tl presence and vice versa.
- "Contains Hg" alone fails: O6Cu1Ba2Hg1Tl1 has Hg but Tc=0
- "Contains Tl" alone fails: O6Cu1Ba2Hg1Tl1 has Tl but Tc=0
- The XOR interaction correctly classifies ALL 18 measured materials

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Hg, no Tl → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Hg, no Tl → **HIGH**
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Tl, no Hg → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Tl, no Hg → **HIGH**
6. O8Cu3Y1Ba1.900La0.100: no Hg, no Tl → **LOW**
7. O8Cu4Y1Ba1.870La0.130: no Hg, no Tl → **LOW**

## Step D: Final Pick

Among predicted HIGH materials:
1. O6.160Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elements
2. O6.270Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elements
3. O4.180Cu1Ba2Hg1: {O,Cu,Ba,Hg} = **4 elements** ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: {O,Ca,Cu,Ba,Tl} = 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: {O,Ca,Cu,Sr,Tl,Pb} = 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among HIGH predictions)
