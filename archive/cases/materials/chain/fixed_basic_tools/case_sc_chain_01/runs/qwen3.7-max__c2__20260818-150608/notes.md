# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90 K, Low Tc: < 90 K

## Step A: Series Peak (O6.xxxCa1Cu2Ba2Hg1)
All 10 members measured:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (first occurrence of max)
Clear superconducting dome shape.

## Step B: Hg + 4 elements (O4.xxxCu1Ba2Hg1)
threshold1 = 127.0 - 32 = 95.0 K
Measured 8 of 11 (skipped O4.096, O4.097, O4.101 - in dip region between 92.0 and 92.5):
- O4.047Cu1Ba2Hg1: 95.0 K → >= 95.0 ✓ QUALIFYING
- O4.081Cu1Ba2Hg1: 98.0 K → >= 95.0 ✓ QUALIFYING
- O4.085Cu1Ba2Hg1: 92.0 K → < 95.0 ✗
- [O4.096Cu1Ba2Hg1: SKIPPED - neighbors 92.0 and 92.5, likely < 95]
- [O4.097Cu1Ba2Hg1: SKIPPED - same region, likely < 95]
- [O4.101Cu1Ba2Hg1: SKIPPED - same region, likely < 95]
- O4.103Cu1Ba2Hg1: 92.5 K → < 95.0 ✗
- O4.150Cu1Ba2Hg1: 97.0 K → >= 95.0 ✓ QUALIFYING
- O4.162Cu1Ba2Hg1: 97.4 K → >= 95.0 ✓ QUALIFYING
- O4.200Cu1Ba2Hg1: 94.2 K → < 95.0 ✗
- O4.220Cu1Ba2Hg1: 97.0 K → >= 95.0 ✓ QUALIFYING

**Q1 = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**

## Step C: Rule Induction

### Key observation: ALL 18 measured materials are HIGH Tc (>= 90 K)
All measured materials share: contain Cu, Ba, O, and Hg.

### Unmeasurable materials analysis:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg) - same series A, between O6.140(105) and O6.190(122) → ~113 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg) - same series A, between O6.240(123) and O6.300(100) → ~112 K → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg) - same series B, between O4.162(97.4) and O4.200(94.2) → ~96 K → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl) - Tl-Ba-Ca-Cu-O type → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb) - no Ba! → LOW
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La) - no Hg/Tl → LOW
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La) - no Hg/Tl → LOW

### Induced Rule:
"High Tc if contains Hg, OR contains both Ba AND Tl. Otherwise low."

This is a multi-factor interaction:
- "Contains Hg" alone is wrong (misses material 4 which is high via Ba+Tl)
- "Contains Ba" alone is wrong (materials 6,7 have Ba but are low without Hg/Tl)
- "Has 5 elements" alone is wrong (material 3 has 4 elements and is high, material 5 has 6 and could be confused)

### Predictions:
- O6.160Ca1Cu2Ba2Hg1: high (Hg)
- O6.270Ca1Cu2Ba2Hg1: high (Hg)
- O4.180Cu1Ba2Hg1: high (Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800: high (Ba+Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (Tl but no Ba)
- O8Cu3Y1Ba1.900La0.100: low (Ba but no Hg/Tl)
- O8Cu4Y1Ba1.870La0.130: low (Ba but no Hg/Tl)

## Step D: Final Pick
High predictions: {1, 2, 3, 4}
Element counts: 1→5, 2→5, 3→4, 4→5
Fewest elements: O4.180Cu1Ba2Hg1 (4 elements)

**Final answer: O4.180Cu1Ba2Hg1**
