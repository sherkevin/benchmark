# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90 K, Low: < 90 K

## Step A - Series Peak (10 measurements used)
Series members (O-Ca-Cu-Ba-Hg, varying O):
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)
Classic dome shape: Tc rises from 92 to 127, then falls to 98.

## Step B - 4-element Hg materials (8 measurements used, skipped 3 extremes)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.047Cu1Ba2Hg1 | (not measured - extreme underdoped) | - |
| O4.200Cu1Ba2Hg1 | (not measured - extreme overdoped) | - |
| O4.220Cu1Ba2Hg1 | (not measured - extreme overdoped) | - |

**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162}** (5 materials with confirmed Tc >= 95)

## Step C - Rule Induction

### All measured materials (18 total) — ALL are HIGH (Tc >= 90)
- Series A (5 elements: O,Ca,Cu,Ba,Hg): 92.0 - 127.0 K, all HIGH
- Series B (4 elements: O,Cu,Ba,Hg): 92.0 - 98.0 K, all HIGH

### Key observation: all measured HIGH materials contain BOTH Ba AND Hg

### Unmeasurable materials analysis:
1. O6.160Ca1Cu2Ba2Hg1 → elements: O,Ca,Cu,Ba,Hg (5 elem, has Ba+Hg) → HIGH
2. O6.270Ca1Cu2Ba2Hg1 → elements: O,Ca,Cu,Ba,Hg (5 elem, has Ba+Hg) → HIGH
3. O4.180Cu1Ba2Hg1 → elements: O,Cu,Ba,Hg (4 elem, has Ba+Hg) → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 → elements: O,Ca,Cu,Ba,Tl (5 elem, has Ba+Tl, no Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → elements: O,Ca,Cu,Sr,Tl,Pb (6 elem, has Tl but NO Ba)
6. O8Cu3Y1Ba1.900La0.100 → elements: O,Cu,Y,Ba,La (5 elem, has Ba but no Hg/Tl)
7. O8Cu4Y1Ba1.870La0.130 → elements: O,Cu,Y,Ba,La (5 elem, has Ba but no Hg/Tl)

### Why single-factor rules fail:
- "Contains Hg → HIGH": wrong on material 4 (Tl-based with Ba, likely HIGH but has no Hg)
- "Has 5 elements → HIGH": wrong on materials 6,7 (5 elements but likely LOW - YBCO overdoped type)
  AND wrong on material 3 (4 elements but HIGH)

### Induced rule (interaction of 2 factors):
**"Contains Ba AND contains (Hg or Tl) → HIGH; otherwise LOW"**

Interaction: BOTH factors required.
- Ba alone isn't sufficient (materials 6,7 have Ba but are LOW — YBCO-type, no heavy charge reservoir)
- Hg or Tl alone isn't sufficient (material 5 has Tl but no Ba, uses Sr instead → lower Tc)

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: Ba✓ Hg✓ → **HIGH** (same family as series A, interpolation ~115 K)
2. O6.270Ca1Cu2Ba2Hg1: Ba✓ Hg✓ → **HIGH** (same family as series A, interpolation ~105 K)
3. O4.180Cu1Ba2Hg1: Ba✓ Hg✓ → **HIGH** (same family as series B, ~96 K)
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba✓ Tl✓ → **HIGH** (Tl-Ba cuprate, Tl-2212 type, Tc ~100 K)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Ba → **LOW** (Sr-based Tl cuprate, lower Tc ~85 K)
6. O8Cu3Y1Ba1.900La0.100: Ba✓ no Hg/Tl → **LOW** (YBCO-type O8, heavily overdoped)
7. O8Cu4Y1Ba1.870La0.130: Ba✓ no Hg/Tl → **LOW** (YBCO-124 type, Tc ~80 K)

## Step D - Final Pick
Among HIGH predictions, element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, no tie to break)

## Budget: 18/18 used
