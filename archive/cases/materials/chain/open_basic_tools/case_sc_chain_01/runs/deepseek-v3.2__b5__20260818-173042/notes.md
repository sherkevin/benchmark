# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 26 units total for both search_db and measure_tc
- High Tc: >= 90 K
- Need to complete 4 steps

## Step A: Find series peak
Find homologous doping series (same element set, varying oxygen). Need to find and measure all series members to identify maximum Tc (T*).

## Initial Strategy
1. First measure seed material Tc ✓ (92.0 K)
2. Search for materials with same elements (Ca, Cu, Ba, Hg, O) to find series
3. Measure all series members to find T*

## Measurements
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (high)

## Homologous Series Measurements:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM - T*)
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K (also maximum)
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

## Step A Result:
Peak composition: O6.210Ca1Cu2Ba2Hg1
T* = 127.0 K

## Budget Tracking
Starting budget: 26 units
Used: 17 units (11 measure_tc + 6 search_db pages)
Remaining: ?

Let me check actual budget usage from last measurement: budget_used was 16, budget_total 26, so remaining = 10 units.

Wait, discrepancy: My calculation says 17 used, but last measurement showed budget_used: 16. Let me trace:
1. Initial measure_tc: budget_used = 1
2. search_db Hg: budget_used = 2  
3. search_db Ca page1: budget_used = 3
4. search_db Ca page2: budget_used = 4
5. search_db Ca page3: budget_used = 5
6. search_db Ca page4: budget_used = 6
7. search_db Ca page5: budget_used = 7
8. measure_tc O6.100: budget_used = 8
9. measure_tc O6.130: budget_used = 9
10. measure_tc O6.140: budget_used = 10
11. measure_tc O6.160: budget_used = 10 (no sample but still counts?)
12. measure_tc O6.190: budget_used = 11
13. measure_tc O6.210: budget_used = 12
14. measure_tc O6.220: budget_used = 13
15. measure_tc O6.240: budget_used = 14
16. measure_tc O6.270: budget_used = 14 (no sample)
17. measure_tc O6.300: budget_used = 15
18. measure_tc O6.310: budget_used = 16

Actually measurement calls without samples still increment budget_used. So total used: 16 units, remaining: 10 units.

## Step B: Filter family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Hg-containing materials with exactly 4 elements (O4.xxxCu1Ba2Hg1 series):
1. O4.047Cu1Ba2Hg1: Measured: 95.0 K (exactly threshold) ✓
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
14. O4.220Cu1Ba2Hg1: Measured: 97.0 K ✓

Based on measurements: O4.047 (lowest O) = 95.0 K, O4.220 (highest O) = 97.0 K.
Likely all O4.xxx have Tc between 95.0-97.0 K, all >= threshold.
Thus Q1 = all 14 O4.xxxCu1Ba2Hg1 materials.

## Budget Status Update
After measuring O4.220 and O4.047: Budget used = 26/26 units (exhausted)

## Step C: Predict unmeasurable materials
Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (from series) - confirmed no sample
2. O6.270Ca1Cu2Ba2Hg1 (from series) - confirmed no sample

Other materials discovered via search_db but not measured (unknown if unmeasurable):
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (Ca search page 5)
- O8Ca0.850Cu2.200Ba2Tl1.800 (Ca search page 5)
- O6Cu1Ba2Hg1Tl1 (Hg search page 9)

These have fractional stoichiometries or different elements. Might be unmeasurable but not confirmed.

Rule induced: All materials containing Hg, Cu, and Ba have Tc >= 90K based on measured data.
Predictions: Both unmeasurable materials predicted HIGH.

## Step D: Final pick
Among unmeasurable materials predicted HIGH (O6.160 and O6.270), both have 5 elements.
O6.160 likely has higher expected Tc based on peak trend (closer to optimal oxygen).
Choose O6.160Ca1Cu2Ba2Hg1.

## Answer prepared in answer.json