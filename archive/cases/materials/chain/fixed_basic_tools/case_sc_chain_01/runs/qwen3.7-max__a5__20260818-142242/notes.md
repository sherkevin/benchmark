# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (all used)
- High Tc: >= 90K, Low: < 90K

## Files
- measurable.txt: 26 compositions (can measure)
- unmeasurable.txt: 7 compositions (cannot measure, must predict)

## Step A Results — Homologous Series Peak (T* = 127.0 K)
| Composition | Tc (K) | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | HIGH |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** | HIGH (PEAK) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** | HIGH (PEAK, tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

**Peak composition**: O6.210Ca1Cu2Ba2Hg1, **T* = 127.0 K**

## Step B — Qualifying 4-element Hg materials (threshold1 = 95.0 K)
Measured:
| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES (= 95.0) |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Inferred from oxygen trend (U-shape, dip near O≈4.10):
- O4.150Cu1Ba2Hg1: ~96.6 K → YES (between 4.101@93.4 and 4.162@97.4)
- O4.200Cu1Ba2Hg1: ~97.1 K → YES (between 4.162@97.4 and 4.220@97.0)
- O4.081Cu1Ba2Hg1: ~94.0 K → NO
- O4.085Cu1Ba2Hg1: ~93.9 K → NO
- O4.096Cu1Ba2Hg1: ~93.6 K → NO
- O4.097Cu1Ba2Hg1: ~93.5 K → NO
- O4.103Cu1Ba2Hg1: ~93.5 K → NO

**Q1 = {O4.047Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**

## Step C — Rule Induction

### All measured data:
HIGH:
- O-Ca-Cu-Ba-Hg (10): all HIGH (92-127 K). Features: Ca=yes, Hg=yes, Tl=no
- O-Cu-Ba-Hg (4): all HIGH (93.4-97.4 K). Features: Ca=no, Hg=yes, Tl=no
- O10Ca2Cu3Ba2Tl2: HIGH (118.97 K). Features: Ca=yes, Hg=no, Tl=yes

LOW:
- O6Cu1Ba2Hg1Tl1: LOW (0 K). Features: Ca=no, Hg=yes, Tl=yes
- O7Cu3Ba2La1: LOW (89.6 K). Features: Ca=no, Hg=no, Tl=no
- O7Cu3Y0.200Ba2La0.800: LOW (81.0 K). Features: Ca=no, Hg=no, Tl=no

### Rule: "HIGH if contains Ca, OR (contains Hg AND does NOT contain Tl)"
Equivalently: the interaction of Ca presence and Hg-without-Tl determines high Tc.

Verification against measured data:
- O-Ca-Cu-Ba-Hg: Ca=yes → HIGH ✓
- O-Cu-Ba-Hg: Ca=no, Hg=yes, Tl=no → HIGH ✓
- O10Ca2Cu3Ba2Tl2: Ca=yes → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Ca=no, Hg=yes, Tl=yes → LOW ✓
- O7Cu3Ba2La1: Ca=no, Hg=no → LOW ✓
- O7Cu3Y0.200Ba2La0.800: Ca=no, Hg=no → LOW ✓

Single-factor checks:
- "contains Hg" wrong on: O6Cu1Ba2Hg1Tl1 (has Hg, is LOW), O10Ca2Cu3Ba2Tl2 (no Hg, is HIGH)
- "has 5 elements" wrong on: O-Cu-Ba-Hg (4 elem, is HIGH), O6Cu1Ba2Hg1Tl1 (5 elem, is LOW)

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Ca=yes → **HIGH** (5 elements)
2. O6.270Ca1Cu2Ba2Hg1: Ca=yes → **HIGH** (5 elements)
3. O4.180Cu1Ba2Hg1: Ca=no, Hg=yes, Tl=no → **HIGH** (4 elements)
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ca=yes → **HIGH** (5 elements)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca=yes → **HIGH** (6 elements)
6. O8Cu3Y1Ba1.900La0.100: Ca=no, Hg=no → **LOW** (5 elements)
7. O8Cu4Y1Ba1.870La0.130: Ca=no, Hg=no → **LOW** (5 elements)

## Step D — Final Pick
Among HIGH predictions (1-5), fewest elements:
- #3 O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- #1,2,4: 5 elements
- #5: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected high Tc ~97 K based on O-Cu-Ba-Hg trend)
