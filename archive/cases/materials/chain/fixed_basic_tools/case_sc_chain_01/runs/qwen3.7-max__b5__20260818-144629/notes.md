# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (all used)
- High Tc: >= 90 K, Low: < 90 K

## Step A Results — Series Peak
Measured all 10 series members (O6.xxxCa1Cu2Ba2Hg1):

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← TIED PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (first occurrence)
Bell curve: rises from 92 K to peak at 127 K (O~6.21-6.22), then drops.

## Step B Results — Family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = **95.0 K**

Measured 8 of 11 family B members (budget: 10 series + 8 family B = 18):

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES (=threshold) |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Unmeasured (3, inferred by interpolation):
- O4.085Cu1Ba2Hg1: between O4.081(98.0) and O4.096(95.6) → ~97.4 → **YES**
- O4.097Cu1Ba2Hg1: very close to O4.096(95.6) → ~95.2 → **YES**
- O4.101Cu1Ba2Hg1: closer to O4.103(92.5) → ~93.4 → **NO**

**Qualifying Q1 (8 materials):**
O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1,
O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

## Step C — Rule Induction

### Key observation: ALL 18 measured materials are HIGH (Tc >= 90 K).
- Series (5 elem, has Hg): Tc range 92.0-127.0 — all HIGH
- Family B (4 elem, has Hg): Tc range 92.5-98.0 — all HIGH

### Element analysis of unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elem, has Hg, Ba
2. O6.270Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elem, has Hg, Ba
3. O4.180Cu1Ba2Hg1: {O,Cu,Ba,Hg} = 4 elem, has Hg, Ba
4. O8Ca0.850Cu2.200Ba2Tl1.800: {O,Ca,Cu,Ba,Tl} = 5 elem, no Hg, has Ba+Tl
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: {O,Ca,Cu,Sr,Tl,Pb} = 6 elem, no Hg, no Ba (has Sr)
6. O8Cu3Y1Ba1.900La0.100: {O,Cu,Y,Ba,La} = 5 elem, no Hg, has Ba, no Tl
7. O8Cu4Y1Ba1.870La0.130: {O,Cu,Y,Ba,La} = 5 elem, no Hg, has Ba, no Tl

### Induced Rule (interaction of multiple factors):
**"HIGH if contains Hg, OR (contains both Ba AND Tl). LOW otherwise."**

Why single-factor rules fail:
- "Contains Hg" → misses material #4 (no Hg, but HIGH due to Ba+Tl synergy)
- "Contains Tl" → wrongly includes material #5 (has Tl but no Ba → LOW; Sr-based Tl cuprates have lower Tc)
- "Contains Ba" → wrongly includes materials #6,#7 (have Ba but no Hg/Tl → YBCO variants with excess O and La doping, likely < 90 K)

The key interaction: Ba and Tl together create a Tl-Ba-Ca-Cu-O type structure with high Tc (~100+ K). Without Ba (using Sr instead), Tl-based cuprates have significantly lower Tc. Without Tl (or Hg), Ba-containing cuprates like YBCO variants may be borderline or low.

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1 → **HIGH** (has Hg; interpolation ~110 K)
2. O6.270Ca1Cu2Ba2Hg1 → **HIGH** (has Hg; interpolation ~112 K)
3. O4.180Cu1Ba2Hg1 → **HIGH** (has Hg; interpolation ~96 K)
4. O8Ca0.850Cu2.200Ba2Tl1.800 → **HIGH** (Ba+Tl; Tl-2212 type, ~100-110 K)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → **LOW** (no Hg, no Ba; (Tl,Pb)-Sr-1212, ~80-85 K)
6. O8Cu3Y1Ba1.900La0.100 → **LOW** (no Hg, no Tl; overdoped YBCO+La, ~85-88 K)
7. O8Cu4Y1Ba1.870La0.130 → **LOW** (no Hg, no Tl; Y-124 variant+La, ~75-80 K)

## Step D — Final Pick
HIGH unmeasurable materials and their element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: **4 elements** ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~96 K)
