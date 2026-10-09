# Notes

## Task Overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Budget Tracking
- Used: 18/18 (exhausted)

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
Series members (12 total, found via search "Ca1Cu2Ba2Hg1"):
1. O6.080Ca1Cu2Ba2Hg1 → Tc = 92 K (measured, HIGH)
2. O6.100Ca1Cu2Ba2Hg1 → NOT MEASURED (budget)
3. O6.130Ca1Cu2Ba2Hg1 → Tc = 102 K (measured, HIGH)
4. O6.140Ca1Cu2Ba2Hg1 → NOT MEASURED (budget)
5. O6.160Ca1Cu2Ba2Hg1 → NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 → Tc = 122 K (measured, HIGH)
7. O6.210Ca1Cu2Ba2Hg1 → Tc = 127 K (measured, HIGH)
8. O6.220Ca1Cu2Ba2Hg1 → Tc = 127 K (measured, HIGH)
9. O6.240Ca1Cu2Ba2Hg1 → Tc = 123 K (measured, HIGH)
10. O6.270Ca1Cu2Ba2Hg1 → NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 → NOT MEASURED (budget)
12. O6.310Ca1Cu2Ba2Hg1 → Tc = 98 K (measured, HIGH)

PEAK: T* = 127 K at both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Selecting O6.220Ca1Cu2Ba2Hg1 as peak composition.

Dome shape: 92 → 102 → 122 → 127 → 127 → 123 → 98

## Step B: 4-element Hg materials with Tc >= threshold1
threshold1 = 127 - 32 = 95 K

Hg search found 27 total materials (3 per page, 9 pages). Seen pages 1-4 (12 materials).
All 12 seen so far are 4-element (O, Cu, Ba, Hg):
Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
Page 4: O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1

Measured 4-element:
- O4.047Cu1Ba2Hg1: Tc = 95.0 K (>= 95, QUALIFIES)
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (>= 95, QUALIFIES)

Remaining 4-element materials NOT measured (budget exhausted).
Remaining 15 Hg materials on pages 5-9 NOT seen (budget exhausted).

## Step C: Unmeasurable materials
Confirmed unmeasurable (tried measure_tc, got "no sample"):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

Prediction rule (multi-factor: element set + oxygen content + dome interpolation):
- 5-element series dome: peaks at ~127 K near O≈6.215, all measured members >= 92 K (all high)
- 4-element series: Tc ~95 K, barely high
- Unmeasurable materials near dome peak should also be high

Predictions:
- O6.160Ca1Cu2Ba2Hg1: Between O6.130 (102) and O6.190 (122), ~112 K → HIGH
- O6.270Ca1Cu2Ba2Hg1: Between O6.240 (123) and O6.310 (98), ~110 K → HIGH

## Step D: Final pick
Both unmeasurables have 5 elements (tied).
Expected Tc: O6.160 ~112 K, O6.270 ~110 K
Pick: O6.160Ca1Cu2Ba2Hg1 (higher expected Tc)
