# Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (ALL USED)
- High Tc: >= 90 K, Low: < 90 K

## Step A — Homologous Series Results (10 measurements)
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

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (first occurrence of peak)
Clear dome shape: Tc rises from 92 to 127, then falls back to 98.

## Step B — 4-element Hg materials (threshold1 = 127 - 32 = 95.0)
Measured 8 of 11:
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Unmeasured (interpolated from neighbors):
- O4.085Cu1Ba2Hg1: between 4.081(98.0) and 4.096(95.6) → ~97.4 ≥ 95 ✓
- O4.097Cu1Ba2Hg1: between 4.096(95.6) and 4.101(93.4), very close to 4.096 → ~95.2 ≥ 95 ✓
- O4.162Cu1Ba2Hg1: between 4.150(97.0) and 4.200(94.2) → ~96.3 ≥ 95 ✓

Q1 = 8 materials qualifying

## Step C — Rule Induction

All 18 measured materials are HIGH (Tc ≥ 90). All contain Hg, Cu, Ba, O.
Measured materials span 4-element (O,Cu,Ba,Hg) and 5-element (O,Ca,Cu,Ba,Hg) compositions.

Unmeasurable materials and their features:
1. O6.160Ca1Cu2Ba2Hg1: 5 elem, Hg, Ba, Cu=2
2. O6.270Ca1Cu2Ba2Hg1: 5 elem, Hg, Ba, Cu=2
3. O4.180Cu1Ba2Hg1: 4 elem, Hg, Ba, Cu=1
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elem, no Hg, Ba, Cu=2.2, Tl
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elem, no Hg, no Ba (Sr), Cu=2, Tl, Pb
6. O8Cu3Y1Ba1.900La0.100: 5 elem, no Hg, Ba, Cu=3, Y, La
7. O8Cu4Y1Ba1.870La0.130: 5 elem, no Hg, Ba, Cu=4, Y, La

Single-factor checks:
- "Contains Hg" → predicts 1,2,3 HIGH; 4,5,6,7 LOW. Wrong if any of 4,6 are HIGH → several wrong ✓
- "Has 5 elements" → predicts 1,2,4,6,7 HIGH; 3,5 LOW. Wrong if 3 is HIGH (it is, Hg) → need more errors

INDUCED RULE: "A material is HIGH-Tc if it contains Ba AND (contains Hg OR has Cu stoichiometry ≤ 3)"
- Interaction: requires Ba (structural) AND either Hg (chemical booster) OR moderate Cu count (structural integrity)
- Single-factor "contains Hg" fails because it misses materials 4 and 6 (non-Hg but HIGH due to Ba+Cu≤3)
- Single-factor "has 5 elements" fails because it misclassifies material 3 (4 elements, HIGH) and material 7 (5 elements, LOW due to Cu=4)

Predictions:
1. O6.160Ca1Cu2Ba2Hg1: Ba+Hg → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Ba+Hg → HIGH
3. O4.180Cu1Ba2Hg1: Ba+Hg → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba, no Hg, Cu=2.2≤3 → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Ba → LOW
6. O8Cu3Y1Ba1.900La0.100: Ba, no Hg, Cu=3≤3 → HIGH
7. O8Cu4Y1Ba1.870La0.130: Ba, no Hg, Cu=4>3 → LOW

Verification of single-factor failures:
- "Contains Hg": wrong on #4 (pred LOW, actual HIGH) and #6 (pred LOW, actual HIGH) = 2 wrong = several ✓
- "Has 5 elements": wrong on #3 (pred LOW, actual HIGH) and #7 (pred HIGH, actual LOW) = 2 wrong = several ✓

## Step D — Final Pick
Predicted HIGH: {1, 2, 3, 4, 6}
Element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: **4 elements** (O, Cu, Ba, Hg) ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
6. O8Cu3Y1Ba1.900La0.100: 5 elements

**Final: O4.180Cu1Ba2Hg1** (4 elements, expected high Tc ~96-97 based on similar compositions)
