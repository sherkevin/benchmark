# Notes - Superconductor Tc Study

## Task Overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High: Tc >= 90 K, Low: Tc < 90 K

## Step A: Find homologous series peak
- Seed elements: O, Ca, Cu, Ba, Hg (5 elements)
- Series = same element set, varying oxygen

## Budget Tracking
- Used: 18/18 (exhausted)
- Note: "no sample" measurements are FREE even when budget exhausted
- Materials with samples return "instrument time exhausted" when budget is 0

## Step A Results - Series Data (O-Ca1-Cu2-Ba2-Hg1 homologous series)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured (PEAK) |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured (PEAK, tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**Peak Tc (T*) = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (tied)
**threshold1 = 127.0 - 32 = 95.0**

## Step B - 4-element Hg materials (O-Cu-Ba-Hg)
From Hg search pages 1-3 (all have samples, couldn't measure due to budget):
1. O4.047Cu1Ba2Hg1 (sample exists)
2. O4.066Cu1Ba2Hg1 (unknown composition error)
3. O4.081Cu1Ba2Hg1 (sample exists)
4. O4.085Cu1Ba2Hg1 (sample exists)
5. O4.096Cu1Ba2Hg1 (sample exists)
6. O4.097Cu1Ba2Hg1 (sample exists)
7. O4.101Cu1Ba2Hg1 (sample exists)
8. O4.103Cu1Ba2Hg1 (sample exists)
9. O4.150Cu1Ba2Hg1 (sample exists)
Hg search had 27 total results (9 pages), only saw 3 pages. Remaining 6 pages have 12 series members + 6 more 4-element materials.
Q1: Cannot determine (no Tc measurements for 4-element materials)

## Step C - Unmeasurable materials discovered
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (interpolation: ~111.8 K, between O6.140=105 and O6.190=122)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (interpolation: ~111.5 K, between O6.240=123 and O6.300=100)
3. O7Ca1Cu2Sr2Tl0.500Pb0.500 → LOW (Tl-based, no Hg, Sr instead of Ba)
4. O8Ca0.850Cu2.200Ba2Tl1.800 → LOW (Tl-based, no Hg, non-stoichiometric Cu)

Rule: HIGH if contains Hg (Hg-based cuprates); LOW for Tl-based without Hg
- Single-factor rules like "≥5 elements → HIGH" or "Cu≥2 → HIGH" are wrong on O7... and O8...

## Step D - Final pick
Among predicted-HIGH unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~111.8
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~111.5
→ Tie on elements (5), tiebreak: higher expected Tc → O6.160Ca1Cu2Ba2Hg1

## Other materials seen in Ca search
- O10Ca2Cu3Ba2Tl2: sample exists, measurable (couldn't measure)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: no sample (unmeasurable)
- O8Ca0.850Cu2.200Ba2Tl1.800: no sample (unmeasurable)
