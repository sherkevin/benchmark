# Superconductor Tc Study Notes

## Initial Setup
- Budget: 34 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as >= 90 K

## Step A - Series Peak
Goal: Find homologous doping series (same element set, varying oxygen) and identify composition with maximum Tc (T*).

Found series: Ca1Cu2Ba2Hg1 with varying oxygen stoichiometry. Measurements:

| Composition | Tc (K) | Notes |
|-------------|--------|-------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Seed, high (>=90) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | High |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | High |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | High |
| O6.160Ca1Cu2Ba2Hg1 | No sample | Unsynthesized |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | High |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | High - MAXIMUM so far |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | High - Tied for MAXIMUM |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | High |
| O6.270Ca1Cu2Ba2Hg1 | No sample | Unsynthesized |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | High |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | High |

Maximum Tc (T*) = 127.0 K
Compositions with maximum Tc: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Need to pick one for Step A answer.

## Budget Tracking
Starting budget: 34 units
Used: 32 units
- Search_db: 13 units
  - Hg search: 7 pages (27 matches)
  - Ca search: 4 pages (15 matches)
  - Cu search: 5 pages (35 matches)
  - Ba search: 1 page (34 matches)
  - Tl search: 2 pages (5 matches)
- Measure_tc: 19 units (19 measurements attempted)
  - 10 measurable materials in O6.xCa1Cu2Ba2Hg1 series (all high, 92.0-127.0 K)
  - 1 measurable: O10Ca2Cu3Ba2Tl2 (high, 118.97 K)
  - 1 measurable: O6Cu1Ba2Hg1Tl1 (low, 0.0 K)
  - 1 measurable: O6Cu1Cd0.250Ba2Tl1.750 (low, 88.0 K)
  - 6 measurements returned "no sample"
Remaining: 2 units

## Final Summary
Step A:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K

Step B:
- Q1 (Hg-containing 4-element materials with Tc >= 95.0 K): ["O4.220Cu1Ba2Hg1"]
- O4.220Cu1Ba2Hg1 measured: Tc = 97.0 K >= 95.0 K
- O4.200Cu1Ba2Hg1 measured: Tc = 94.2 K < 95.0 K (does not qualify)
- Other O_xCu1Ba2Hg1 materials not measured due to budget constraints

Step C:
- Rule induced: "Materials are low Tc if they contain Tl but not Ca; otherwise high Tc."
- Fits all measurable materials:
  - O6Cu1Ba2Hg1Tl1 (Tl, no Ca): low (0.0 K) ✓
  - O6Cu1Cd0.250Ba2Tl1.750 (Tl, no Ca): low (88.0 K) ✓
  - O4.200Cu1Ba2Hg1 (no Tl): high (94.2 K) ✓
  - O4.220Cu1Ba2Hg1 (no Tl): high (97.0 K) ✓
  - O10Ca2Cu3Ba2Tl2 (Tl, Ca): high (118.97 K) ✓
  - O6.xCa1Cu2Ba2Hg1 series (no Tl): all high ✓
- Predictions for 18 unmeasurable materials (all predicted high)

Step D:
- Final pick: O6.160Ca1Cu2Ba2Hg1
- Reason: Among unmeasurable materials predicted high, has 5 elements (tied with others), expected Tc higher than O6.270Ca1Cu2Ba2Hg1 based on series trend.