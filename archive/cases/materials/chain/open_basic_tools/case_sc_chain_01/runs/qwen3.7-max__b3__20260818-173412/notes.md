# Notes - Superconductor Tc Study

## Task Overview
- Budget: 26 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K

## Step A: Series peak
Series: OxCa1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
Members found (12 total):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: UNMEASURABLE (no sample)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K *** PEAK ***
- O6.220Ca1Cu2Ba2Hg1: 127.0 K *** PEAK ***
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: UNMEASURABLE (no sample)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

T* = 127.0 K, peak composition: O6.210Ca1Cu2Ba2Hg1 (tie with O6.220)

## Step B: 4-element Hg materials (OxCu1Ba2Hg1)
threshold1 = 127.0 - 32 = 95.0 K
14 materials found (all OxCu1Ba2Hg1):
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
Measured: O4.101 (93.4 K), O4.200 (94.2 K) — both below 95 K
Q1 = [] (none measured above 95 K)

## Step C: Unmeasurable materials
Confirmed: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
Rule: Multi-factor parabolic interpolation within homologous series
- O6.160: between O6.140 (105) and O6.190 (122) → ~112 K → high
- O6.270: between O6.240 (123) and O6.300 (100) → ~112 K → high

## Step D: Final pick
Both predicted high with 5 elements. Tie-break: higher expected Tc.
- O6.160: ~111.8 K (interpolated)
- O6.270: ~111.5 K (interpolated)
Final: O6.160Ca1Cu2Ba2Hg1

## Other materials found (not tested)
- O10Ca2Cu3Ba2Tl2 (5 elem, no Hg)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg)
- O6Cu1Ba2Hg1Tl1 (5 elem: O, Cu, Ba, Hg, Tl)

## Budget used: 26/26
- Searches: Ca (5 pages) + Hg (9 pages) = 14
- Measurements: 14 attempts (12 successful + 2 no sample)
