# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low Tc: < 90 K
- Budget: 34 measure_tc calls (used 26)

## Step A — Homologous series results
| Composition | Tc (K) | High? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Y |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Y |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Y |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Y |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Y |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Y |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Y |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Y |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Y |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Y |

**Peak: T* = 127.0 K** (O6.210Ca1Cu2Ba2Hg1, tied with O6.220Ca1Cu2Ba2Hg1)

## Step B — 4-element Hg materials
threshold1 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |

**Q1 = 7 materials** with Tc >= 95 K

## Other measured materials (for rule building)
| Composition | Elements | #elem | Hg? | Tl? | Ca? | Tc (K) | High? |
|---|---|---|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl | 5 | Y | Y | N | 0.0 | N |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl | 5 | N | Y | Y | 118.97 | Y |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl | 5 | N | Y | N | 88.0 | N |
| O7Cu3Ba2La1 | O,Cu,Ba,La | 4 | N | N | N | 89.6 | N |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La | 5 | N | N | N | 81.0 | N |

## Step C — Rule induction

### Analysis of HIGH vs LOW:
HIGH materials all have: Hg (without Tl) or Ca
LOW materials all lack both: no Hg-without-Tl AND no Ca

Key observations:
- All Hg-only materials (no Tl) are HIGH (both 4-elem and 5-elem series)
- O6Cu1Ba2Hg1Tl1 has Hg AND Tl → LOW (Tc=0), so "contains Hg" alone fails
- O10Ca2Cu3Ba2Tl2 has Ca and Tl (no Hg) → HIGH, so Ca also enables high Tc
- O6Cu1Cd0.250Ba2Tl1.750 has Tl but no Hg and no Ca → LOW
- O7Cu3Ba2La1 has no Hg, no Ca → LOW
- O7Cu3Y0.200Ba2La0.800 has no Hg, no Ca → LOW

**Rule: High Tc if material contains Hg without Tl, OR contains Ca.**
Equivalently: (Hg AND NOT Tl) OR (has Ca)

This is a multi-factor interaction rule (not single-factor).

### Predictions for unmeasurable materials:
| Composition | Elements | Hg? | Tl? | Ca? | Prediction |
|---|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | 5: O,Ca,Cu,Ba,Hg | Y | N | Y | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | 5: O,Ca,Cu,Ba,Hg | Y | N | Y | HIGH |
| O4.180Cu1Ba2Hg1 | 4: O,Cu,Ba,Hg | Y | N | N | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5: O,Ca,Cu,Ba,Tl | N | Y | Y | HIGH |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6: O,Ca,Cu,Sr,Tl,Pb | N | Y | Y | HIGH |
| O8Cu3Y1Ba1.900La0.100 | 5: O,Cu,Y,Ba,La | N | N | N | LOW |
| O8Cu4Y1Ba1.870La0.130 | 5: O,Cu,Y,Ba,La | N | N | N | LOW |

## Step D — Final pick
Among HIGH predictions, fewest elements:
- O4.180Cu1Ba2Hg1: 4 elements ← WINNER (fewest)
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1**
