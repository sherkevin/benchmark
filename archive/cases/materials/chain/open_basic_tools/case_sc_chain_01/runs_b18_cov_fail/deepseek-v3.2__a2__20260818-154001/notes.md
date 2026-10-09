# Superconductor Tc Study Notes

## Initial Information
- Starting budget: 18 units (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as: Tc >= 90 K
- Task has 4 steps (A, B, C, D)
- Each search_db page costs 1 unit, each measure_tc costs 1 unit

## Budget Tracking
Starting budget: 18 units
Used: 18 units (search for CaCuBaHg, search for Hg pages 1-4, search for Ca pages 1-5, 8 measurements)
Remaining: 0 units

## Budget exhausted - cannot make any more searches or measurements

## Final Answer Summary
- Step A: Peak composition = O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K
- Threshold1 = 95.0 K
- Step B: Qualifying set Q1 = [] (no Hg-containing materials with exactly 4 elements found to have Tc ≥ 95.0 K)
- Step C: Predictions for all unmeasurable materials discovered
- Step D: Final pick = O6.160Ca1Cu2Ba2Hg1 (among predicted high, has 5 elements, expected higher Tc than others due to oxygen content closer to peak)
- Rule: Materials containing Ca have high Tc (≥90 K) if oxygen content is between 6.080 and 6.310; materials without Ca have low Tc (<90 K)

## Limitations
- Budget exhausted early, preventing comprehensive searches and measurements
- Could not verify Tc of O4.xxxCu1Ba2Hg1 materials
- Rule may be oversimplified (single-factor elements)

## Search Results
- Search for "CaCuBaHg" returned 0 results on page 1
- Search for "Hg" page 1 returned 3 results: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- Search for "Hg" page 2 returned 3 results: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- Search for "Hg" page 3 returned 3 results: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
- There are 27 total matches for "Hg", has_more: true after page 3
- Search for "Ca" page 1 returned 3 results: O10Ca2Cu3Ba2Tl2, O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1
- Search for "Ca" page 2 returned 3 results: O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1
- Search for "Ca" page 3 returned 3 results: O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1
- Search for "Ca" page 4 returned 3 results: O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1
- Search for "Ca" page 5 returned 3 results: O6.310Ca1Cu2Ba2Hg1, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800
- There are 15 total matches for "Ca", has_more: false after page 5

## Homologous Doping Series (Step A) - Measurements
Based on search results, the homologous doping series appears to be:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - NOT MEASURED
3. O6.130Ca1Cu2Ba2Hg1 - NOT MEASURED
4. O6.140Ca1Cu2Ba2Hg1 - NOT MEASURED
5. O6.160Ca1Cu2Ba2Hg1 - NOT MEASURED
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1 - NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - NOT MEASURED
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

## Step A Analysis
Maximum Tc found so far: 127.0 K at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 (choosing this as it comes first)