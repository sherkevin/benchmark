# Superconductor Tc Study Notes

## Initial Setup
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- "High" Tc defined as Tc >= 90 K

## Plan
1. **Step A**: Find and measure homologous doping series members (same element set, varying oxygen)
   - Search for materials with elements Ca, Cu, Ba, Hg (oxygen varies)
   - Measure Tc of all found members to find maximum Tc (T*)

2. **Step B**: Filter materials containing Hg with exactly 4 elements
   - threshold1 = T* - 32
   - Find all such materials with Tc >= threshold1 (set Q1)

3. **Step C**: Induce rule from measurable materials to predict high/low for unmeasurable ones
   - Search for unmeasurable materials (those with "no sample exists")
   - Develop rule based on measurable materials
   - Predict high/low for all unmeasurable materials found

4. **Step D**: Among unmeasurable materials predicted high, choose one with fewest elements
   - Tie-break by higher expected Tc

## Step A: Homologous Series Members Found
Found 12 materials in the homologous series (same element set: Ca, Cu, Ba, Hg, varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

## Step A: Homologous Series Measurements
Measured Tc values:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc (T*): 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1 - need to check which one to report)

## Step B: Materials with Hg and exactly 4 elements
threshold1 = T* - 32 = 127 - vii - 95 K

Found from Hg search page 1:
1. O4.047Cu1Ba2Hg1: Tc = 95.0 K (qualifies: Tc >= 95 K)
2. O4.066Cu1Ba2Hg1: Not measured (no budget left, and need to search first)
3. O4.081Cu1Ba2Hg1: Tc = 98.0 K (qualifies: Tc >= 95 K)

Qualifying set Q1: [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1]

## Step C: Unmeasurable materials
Found via search_db while searching for Ca-containing materials:
1. O6.160Ca1Cu2Ba2Hg1 (unmeasurable)
2. O6.270Ca1Cu2Ba2Hg1 (unmeasurable)

Rule induced: All measurable materials found contain Hg, Cu, and Ba, and all have Tc >= 90 K (high).
Prediction: Both unmeasurable materials also contain Hg, Cu, and Ba, so predicted high.

## Step D: Final pick
Both unmeasurable materials have 5 elements (O, Ca, Cu, Ba, Hg). Tie.
Based on interpolation of Tc vs oxygen content:
- O6.160 between O6.140 (105 K) and O6.190 (122 K): expected Tc ~115 K
- O6.270 between O6.240 (123 K) and O6.300 (100 K): expected Tc ~110 K
Choose O6.160Ca1Cu2Ba2Hg1 as final (higher expected Tc).

## Budget Tracking
Total budget: 18 units
Used: 18 units (search_db: 6 pages, measure_tc: 12 measurements)
Remaining: 0 units