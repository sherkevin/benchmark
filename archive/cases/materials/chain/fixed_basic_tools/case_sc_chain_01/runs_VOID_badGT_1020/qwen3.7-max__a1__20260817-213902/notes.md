# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls (used 25, 9 remaining)
- High Tc: >= 90 K, Low: < 90 K

## Step A — Series Peak
Homologous series O6.xxxCa1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg):

| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | HIGH |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | HIGH |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | HIGH |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

**Peak: O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K** (tied with O6.220 at 127.0, picking first)

## Step B — Family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = 95.0 K

O4.xxxCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg):

| Composition | Tc (K) | >= 95.0? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | - |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | - |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

No other measurable materials have Hg + exactly 4 elements:
- O6Cu1Ba2Hg1Tl1: 5 elements (O,Cu,Ba,Hg,Tl)
- O6.xxxCa1Cu2Ba2Hg1: 5 elements (O,Ca,Cu,Ba,Hg)

## Other measurable materials (for rule induction)

| Composition | Tc (K) | High/Low | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW | O,Cu,Ba,Hg,Tl (5) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | HIGH | O,Ca,Cu,Ba,Tl (5) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | LOW | O,Cu,Cd,Ba,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | LOW | O,Cu,Ba,La (4) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | LOW | O,Cu,Y,Ba,La (5) |

## Step C — Rule Induction

### Analysis of HIGH vs LOW:

HIGH materials (Tc >= 90 K):
- O6.xxxCa1Cu2Ba2Hg1 (10 entries): Hg=yes, Tl=no → HIGH
- O4.xxxCu1Ba2Hg1 (10 entries): Hg=yes, Tl=no → HIGH  
- O10Ca2Cu3Ba2Tl2: Tl=yes, Hg=no, Ca=yes → HIGH

LOW materials (Tc < 90 K):
- O6Cu1Ba2Hg1Tl1: Hg=yes, Tl=yes → LOW (Tc=0!)
- O6Cu1Cd0.250Ba2Tl1.750: Tl=yes, Hg=no, Ca=no → LOW
- O7Cu3Ba2La1: Hg=no, Tl=no → LOW
- O7Cu3Y0.200Ba2La0.800: Hg=no, Tl=no → LOW

### Induced Rule:
**HIGH if contains exactly one of {Hg, Tl} (XOR) AND does not contain La or Cd.**
Equivalently: HIGH if (Hg and not Tl) or (Tl and Ca and not Hg), AND not La, not Cd.

Verification against all 25 measured: ✓ all correct.

### Unmeasurable materials and predictions:

| Composition | Elements | Hg | Tl | La | Cd | Prediction |
|---|---|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | O,Ca,Cu,Ba,Hg (5) | yes | no | no | no | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | O,Ca,Cu,Ba,Hg (5) | yes | no | no | no | HIGH |
| O4.180Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | yes | no | no | no | HIGH |
| O4.047Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | yes | no | no | no | HIGH |
| O4.066Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | yes | no | no | no | HIGH |
| O4.160Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | yes | no | no | no | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | O,Ca,Cu,Ba,Tl (5) | no | yes | no | no | HIGH |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | O,Ca,Cu,Sr,Tl,Pb (6) | no | yes | no | no | HIGH |
| O8Cu3Y1Ba1.900La0.100 | O,Cu,Y,Ba,La (5) | no | no | yes | no | LOW |
| O8Cu4Y1Ba1.870La0.130 | O,Cu,Y,Ba,La (5) | no | no | yes | no | LOW |

## Step D — Final Pick

HIGH predictions: 8 materials
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4 (four tied materials). Break tie by higher expected Tc.

Expected Tc estimates (from nearby measured O4.xxx series):
- O4.047: ~93-96 K (below measured range, extrapolating from O4.081=98.0)
- O4.066: ~95-97 K (between O4.047 and O4.081)
- O4.160: ~97 K (very close to O4.162=97.4)
- O4.180: ~96 K (between O4.162=97.4 and O4.200=94.2)

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, expected Tc ~97 K, highest among tied)
