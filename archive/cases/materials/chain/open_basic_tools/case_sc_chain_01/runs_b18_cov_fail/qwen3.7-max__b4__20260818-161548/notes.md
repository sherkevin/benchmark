# Notes - Superconductor Tc Study

## Budget: 18/18 used

## Seed: O6.080Ca1Cu2Ba2Hg1 → Tc = 92.0 K

## Step A - Homologous Series (O, Ca, Cu, Ba, Hg with varying oxygen)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | ? | not measured (budget) |
| O6.140Ca1Cu2Ba2Hg1 | ? | not measured (budget) |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | ? | not measured (budget) |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**Peak: O6.220Ca1Cu2Ba2Hg1, T* = 127.0 K**
- O6.130/O6.140 likely between 92.5-122.0 (< 127)
- O6.300 likely between 98-123 (< 127)

## Step B - 4-element Hg materials (O, Cu, Ba, Hg), threshold = 95
| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES (>= 95) |
| O4.066Cu1Ba2Hg1 | ERROR | "unknown composition" - not valid |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | ? | not measured (budget) |

Note: Hg search has 27 total materials, only saw 6 (pages 1-2 of 9)

**Q1 = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1}**

## Step C - Unmeasurable materials
Confirmed unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - interpolation: ~112 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - interpolation: ~112 K → HIGH

Rule (multi-factor): Materials containing Cu-O planes AND Ba charge reservoirs AND Hg layers with appropriate oxygen doping are high-Tc (>=90K). Both unmeasurable materials have all required features.

## Step D - Final pick
Both unmeasurable have 5 elements. Tie-break by expected Tc:
- O6.160: ~112.2 K (interpolated between 92.5 and 122.0)
- O6.270: ~112.3 K (interpolated between 123.0 and 98.0)
→ O6.270Ca1Cu2Ba2Hg1 (slightly higher expected Tc)

## Other materials found (not tested):
- O10Ca2Cu3Ba2Tl2 (5 elem, no Hg)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg)
