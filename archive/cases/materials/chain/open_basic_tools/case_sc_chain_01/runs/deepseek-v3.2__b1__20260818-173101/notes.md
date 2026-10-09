# Superconductor Tc Study Notes

## Summary
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 26 units total (shared between search_db and measure_tc)
- Goal: Complete 4 steps (A-D) as described in TASK.md

## Step A - Series Peak
Need to find homologous doping series members (same element set, varying oxygen).
Seed: O6.080Ca1Cu2Ba2Hg1

Plan:
1. Search for materials containing Hg to find series members
2. Search for materials containing Cu, Ba, Ca to find more
3. Identify series members (same element set except oxygen)
4. Measure Tc for all series members to find maximum

## Budget Status
Budget exhausted: 26/26 units used
Cannot perform any more searches or measurements

## Data Collected

### Step A - Series Measurements (O_xCa1Cu2Ba2Hg1)
Measured Tc:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (peak)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (also peak)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Unmeasurable (no sample):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

Peak Tc (T*): 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 (choose first occurrence)

### Step B - Hg-containing materials with exactly 4 elements
Found 14 O_xCu1Ba2Hg1 materials (4 elements: O, Cu, Ba, Hg):
1. O4.047Cu1Ba2Hg1 - Measured: Tc = 95.0 K (meets threshold1 = 95 K)
2. O4.066Cu1Ba2Hg1 - Not measured (budget exhausted)
3. O4.081Cu1Ba2Hg1 - Not measured
4. O4.085Cu1Ba2Hg1 - Not measured
5. O4.096Cu1Ba2Hg1 - Not measured
6. O4.097Cu1Ba2Hg1 - Not measured
7. O4.101Cu1Ba2Hg1 - Not measured
8. O4.103Cu1Ba2Hg1 - Not measured
9. O4.150Cu1Ba2Hg1 - Not measured
10. O4.160Cu1Ba2Hg1 - Not measured
11. O4.162Cu1Ba2Hg1 - Not measured
12. O4.180Cu1Ba2Hg1 - Not measured
13. O4.200Cu1Ba2Hg1 - Not measured
14. O4.220Cu1Ba2Hg1 - Not measured

Threshold1 = T* - 32 = 127 - 32 = 95 K

Only know that O4.047Cu1Ba2Hg1 meets threshold (Tc = 95.0 K).
Don't know about others.

### Step C - Unmeasurable materials
Found so far:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)

Need to predict high/low for these based on rule induced from measurable materials.

### Step D - Final pick
Among unmeasurable materials predicted high, choose one with fewest elements.
Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 (both 5 elements).
Choose O6.160Ca1Cu2Ba2Hg1 as it likely has higher Tc (between 105-122 K vs 123-100 K).

## Reflection on Strategy
1. Used budget inefficiently by searching too many Hg pages (9 pages = 9 units)
2. Should have been more strategic: measure series members first, calculate threshold, then selectively measure O_xCu1Ba2Hg1 materials
3. Could have searched for "O4." pattern to find O_xCu1Ba2Hg1 materials more efficiently
4. Budget management is critical - each search page and measurement costs 1 unit
5. Need to plan all steps before starting to use budget

## Final Answer Summary
Created answer.json with:
- Peak: O6.210Ca1Cu2Ba2Hg1 (Tc=127 K)
- Threshold: 95 K (127 - 32)
- Qualifying: Only O4.047Cu1Ba2Hg1 known to meet threshold (Tc=95 K)
- Predictions: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 predicted "high"
- Final: O6.160Ca1Cu2Ba2Hg1 (fewest elements tie-break: higher expected Tc)
- Rule: Based on oxygen content trend in O_xCa1Cu2Ba2Hg1 series

Limitations:
1. Only measured 1 of 14 O_xCu1Ba2Hg1 materials due to budget exhaustion
2. Don't know if other O_xCu1Ba2Hg1 materials are unmeasurable
3. Don't know if other O_xCu1Ba2Hg1 materials have Tc >= 95 K
4. Rule is single-factor (oxygen content) which task warns may be wrong

## Search Results
1. Hg search page 1:
   - O4.047Cu1Ba2Hg1
   - O4.066Cu1Ba2Hg1
   - O4.081Cu1Ba2Hg1
   - Has more pages: true
   - Note: 3 of 27 matches shown

2. Hg search page 2:
   - O4.085Cu1Ba2Hg1
   - O4.096Cu1Ba2Hg1
   - O4.097Cu1Ba2Hg1
   - Has more pages: true

3. Ca search page 1:
   - O10Ca2Cu3Ba2Tl2
   - O6.080Ca1Cu2Ba2Hg1 (seed)
   - O6.100Ca1Cu2Ba2Hg1
   - Has more pages: true
   - Note: 3 of 15 matches shown

4. Ca search page 2:
   - O6.130Ca1Cu2Ba2Hg1
   - O6.140Ca1Cu2Ba2Hg1
   - O6.160Ca1Cu2Ba2Hg1
   - Has more pages: true

5. Ca search page 3:
   - O6.190Ca1Cu2Ba2Hg1
   - O6.210Ca1Cu2Ba2Hg1
   - O6.220Ca1Cu2Ba2Hg1
   - Has more pages: true

6. Ca search page 4:
   - O6.240Ca1Cu2Ba2Hg1
   - O6.270Ca1Cu2Ba2Hg1
   - O6.300Ca1Cu2Ba2Hg1
   - Has more pages: true

7. Ca search page 5:
   - O6.310Ca1Cu2Ba2Hg1
   - O7Ca1Cu2Sr2Tl0.500Pb0.500
   - O8Ca0.850Cu2.200Ba2Tl1.800
   - Has more pages: false
   - Note: Last page of Ca search

## Step A Results - Series Measurements
Measured Tc for O_xCa1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (high)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (high)
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (high)
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (high)
5. O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (high)
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (high)
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (high)
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (high)
10. O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (high)
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (high)

Maximum Tc (T*): 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Need to pick one as peak composition - choose O6.210Ca1Cu2Ba2Hg1 (first occurrence).

Unmeasurable materials found so far (for Step C):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1