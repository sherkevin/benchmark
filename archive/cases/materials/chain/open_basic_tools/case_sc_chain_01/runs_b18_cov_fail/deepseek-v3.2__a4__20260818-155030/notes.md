# Superconductor Tc Study Notes

## Task Overview
- Total budget: 18 units (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Goal: Complete 4 dependent steps (A-D)

## Step A: Series Peak
Find homologous doping series (same element set, varying oxygen) and identify composition with maximum Tc = T*.

## Step B: Filter Family B
Find materials containing Hg with exactly 4 elements, report all with Tc >= threshold1 = T* - 32.

## Step C: Extrapolate
Induce rule from measurable materials, predict high/low for unmeasurable ones.

## Step D: Final Pick
Among unmeasurable materials predicted high, choose one with fewest elements (tie-break by higher expected Tc).

## Initial Observations
- Seed: O6.080Ca1Cu2Ba2Hg1 has 5 elements (O, Ca, Cu, Ba, Hg)
- Oxygen content is fractional: 6.080
- Need to find series with same element set (Ca, Cu, Ba, Hg) but varying oxygen

## Budget Planning
Total: 18 units
Need to allocate wisely between search_db (to find materials) and measure_tc (to measure Tc).

## Budget Tracking
Used: 18 units (8 search_db + 10 measure_tc)
Remaining: 0 units

## Complete Homologous Series Measurements
All Tc values:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: Unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: Unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

## Step A Conclusion
Maximum Tc (T*) = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of peak value)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step A Analysis
Peak Tc (T*) appears to be 127.0 K at either O6.210 or O6.220.
Based on trend: increasing from O6.080 (92.0) to O6.210 (127.0), then decreasing to O6.310 (98.0).
Both O6.210 and O6.220 have same Tc (127.0). Need to determine which is actual peak.

## Unmeasurable Materials Found
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
Need to check O6.140 might also be unmeasurable?

## Final Analysis

### Step A Completed
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K

### Step B Limitations
No budget remaining to measure Tc of 4-element Hg materials. Based on oxygen content trend:
- Series materials (oxygen ~6.0-6.3): Tc mostly > 90 K
- 4-element materials (oxygen ~4.0-4.15): Likely lower Tc, possibly < 95.0 K
- Without measurements, cannot definitively identify qualifying materials

### Step C Predictions
Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 - Likely high (Tc between 105.0 and 122.0 K)
2. O6.270Ca1Cu2Ba2Hg1 - Likely high (Tc between 123.0 and 100.0 K)

Rule induced: Materials in Ca-Cu-Ba-Hg-O system with oxygen content 6.0-6.3 have high Tc (≥90 K).

### Step D Final Pick
Among unmeasurable materials predicted high:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
Both have same number of elements. Choose O6.160 as closer to rising side of Tc curve.

### Lessons Learned
1. Budget allocation critical - should have saved units for Step B measurements
2. Binary search approach for series peak would save measurement budget
3. Need to search more broadly for different material types
4. Inducing rules requires diverse measurable materials