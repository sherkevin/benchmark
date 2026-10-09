# Superconductor Tc Study Notes

## Initial Status
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Tc = 92.0 K (high, ≥ 90 K)
- Budget: — used, — remaining out of 26 total

## Step A: Find homologous doping series
Seed belongs to homologous doping series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen.

Found 12 materials in the series via Ca search (pages 1-5). Measured Tc:
1. O6.080Ca1Cu2Ba2Hg1 (seed): Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) among measurable materials = 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1).
Budget used: 24 (16 search_db + 8 measure_tc on available samples). Budget remaining: 2.

## Step B: Find materials with Hg and exactly 4 elements
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Found 14 materials with exactly 4 elements (O, Cu, Ba, Hg) from Hg search pages 1-5:
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

Need to measure their Tc to find which have Tc ≥ 95.0 K, but only 2 budget units remaining! Strategy needed.

Measured O4.047Cu1Ba2Hg1: Tc = 95.0 K exactly at threshold! Qualifies for Q1.
Budget now exhausted (26/26).

## Step C: Induce rule from measurable materials
Measured data for Ca series (O, Ca, Cu, Ba, Hg):
- O6.080: 92.0 K (low)
- O6.100: 92.5 K (low)
- O6.130: 102.0 K (high)
- O6.140: 105.0 K (high)
- O6.190: 122.0 K (high)
- O6.210: 127.0 K (high)
- O6.220: 127.0 K (high)
- O6.240: 123.0 K (high)
- O6.300: 100.0 K (high)
- O6.310: 98.0 K (high)

Unmeasurable in Ca series: O6.160 and O6.270

Measured for 4-element: O4.047Cu1Ba2Hg1: 95.0 K (high)

Pattern: For Ca-series, oxygen ≥ 6.130 gives high Tc. For 4-element series, O4.047 gives high Tc (95K at threshold).

Rule: Two thresholds based on element set:
- Ca-Cu-Ba-Hg-O: high if oxygen ≥ 6.130
- Cu-Ba-Hg-O (4 elements): high if oxygen ≥ 4.047

## Predictions:
- O6.160Ca1Cu2Ba2Hg1: oxygen 6.160 ≥ 6.130 → high
- O6.270Ca1Cu2Ba2Hg1: oxygen 6.270 ≥ 6.130 → high
- O4.066Cu1Ba2Hg1: oxygen 4.066 ≥ 4.047 → high (assuming same rule as O4.047)

## Step D: Choose unmeasurable material predicted high with fewest elements
- O6.160 and O6.270: both 5 elements, both predicted high
- Tie breaker: higher expected Tc
- O6.160: expected Tc ~111.8K (between O6.140 105K and O6.190 122K)
- O6.270: expected Tc ~111.5K (between O6.240 123K and O6.300 100K)
- Choose O6.160Ca1Cu2Ba2Hg1 (slightly higher expected Tc)