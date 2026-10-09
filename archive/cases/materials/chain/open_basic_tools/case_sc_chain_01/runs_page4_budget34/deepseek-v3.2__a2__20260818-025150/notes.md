# Superconductor Tc Study Notes

## Task Overview
- Budget: 34 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K
- Need to complete 4 steps: A, B, C, D

## Initial Analysis
The seed belongs to a homologous doping series with same element set, varying oxygen. Need to find series members by searching for materials with similar composition patterns.

## Budget Tracking
Total budget: 34 units
Used: 34 units (search_db "Hg" pages 1-7, search_db "Ca" pages 1-4, search_db "Cu" pages 1-2, measure_tc x at least 24)
Remaining: 0 units

## Search Results - Hg-containing materials (page 1)
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
Total matches: 27, has_more: true

## Search Results - Ca-containing materials (page 1) - Found series members!
1. O10Ca2Cu3Ba2Tl2 (different - has Tl)
2. O6.080Ca1Cu2Ba2Hg1 (seed)
3. O6.100Ca1Cu2Ba2Hg1 
4. O6.130Ca1Cu2Ba2Hg1
Total matches: 15, has_more: true

## Complete Series Members (same element set: O, Ca, Cu, Ba, Hg) - Measurement Results
From Ca search results:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable) - MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable) - Also maximum
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K (measurable)

## Step A Results
Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)

## Step B Analysis
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= 95.0 K

### 4-element Hg-containing materials found (Complete measurements):
1. O4.047Cu1Ba2Hg1 - Unmeasurable (no sample)
2. O4.066Cu1Ba2Hg1 - Unmeasurable (no sample)
3. O4.081Cu1Ba2Hg1 - Tc = 98.0 K ✓ (>= 95.0) - Q1 member
4. O4.085Cu1Ba2Hg1 - Tc = 92.0 K ✗ (< 95.0)
5. O4.096Cu1Ba2Hg1 - Tc = 95.6 K ✓ (>= 95.0) - Q1 member
6. O4.097Cu1Ba2Hg1 - Tc = 95.3 K ✓ (>= 95.0) - Q1 member
7. O4.101Cu1Ba2Hg1 - Tc = 93.4 K ✗ (< 95.0)
8. O4.103Cu1Ba2Hg1 - Tc = 92.5 K ✗ (< 95.0)
9. O4.150Cu1Ba2Hg1 - Tc = 97.0 K ✓ (>= 95.0) - Q1 member
10. O4.160Cu1Ba2Hg1 - Unmeasurable (no sample)
11. O4.162Cu1Ba2Hg1 - Tc = 97.4 K ✓ (>= 95.0) - Q1 member
12. O4.180Cu1Ba2Hg1 - Unmeasurable (no sample)
13. O4.200Cu1Ba2Hg1 - Tc = 94.2 K ✗ (< 95.0)
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K ✓ (>= 95.0) - Q1 member

### Step B Results - Q1 Set (materials with Tc >= 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.096Cu1Ba2Hg1 (95.6 K)
3. O4.097Cu1Ba2Hg1 (95.3 K)
4. O4.150Cu1Ba2Hg1 (97.0 K)
5. O4.162Cu1Ba2Hg1 (97.4 K)
6. O4.220Cu1Ba2Hg1 (97.0 K)

## Additional Measurement
O10Ca2Cu3Ba2Tl2 - Tc = 118.97 K (>= 90, so high)

## Step C Analysis - Inducing a Rule
I need to induce a rule from measurable materials to predict high/low for unmeasurable ones.

### All Measurable Materials Found (all have Tc >= 90 K):
1. Series O_xCa1Cu2Ba2Hg1: All have Tc >= 92.0 K
2. Series O_xCu1Ba2Hg1: All have Tc >= 92.0 K
3. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K

### Unmeasurable Materials Found:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. O4.066Cu1Ba2Hg1 (4 elements)
5. O4.160Cu1Ba2Hg1 (4 elements)
6. O4.180Cu1Ba2Hg1 (4 elements)
7. O6Cu1Ba2Hg1Tl1 (5 elements - from search results)

### Induced Rule:
"Materials are high-Tc (Tc >= 90K) if they contain both Cu and Ba and have oxygen stoichiometry greater than 4.0"

This rule fits all measurable materials:
- All contain Cu and Ba
- All have oxygen > 4.0
- All are high-Tc

### Predictions for Unmeasurable Materials:
All unmeasurable materials satisfy the rule:
1. O6.160Ca1Cu2Ba2Hg1: Contains Cu and Ba, oxygen=6.160 > 4.0 → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Cu and Ba, oxygen=6.270 > 4.0 → HIGH
3. O4.047Cu1Ba2Hg1: Contains Cu and Ba, oxygen=4.047 > 4.0 → HIGH
4. O4.066Cu1Ba2Hg1: Contains Cu and Ba, oxygen=4.066 > 4.0 → HIGH
5. O4.160Cu1Ba2Hg1: Contains Cu and Ba, oxygen=4.160 > 4.0 → HIGH
6. O4.180Cu1Ba2Hg1: Contains Cu and Ba, oxygen=4.180 > 4.0 → HIGH
7. O6Cu1Ba2Hg1Tl1: Contains Cu and Ba, oxygen=6 > 4.0 → HIGH

## Step D Analysis
Among unmeasurable materials predicted as HIGH, choose one with:
1. Fewest elements
2. If tie, higher expected Tc

Materials with fewest elements (4 elements):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

Based on neighboring measurements and expected Tc, O4.066Cu1Ba2Hg1 is chosen as final answer.

### Observations:
- Some materials are unmeasurable (no sample)
- Measurable materials show Tc around 92-98 K
- Need to measure more to find all with Tc >= 95.0 K

## Step A Plan
1. Search for materials containing all elements from seed (Ca, Cu, Ba, Hg, O) but with different oxygen stoichiometry
2. Measure Tc of all series members to find maximum Tc (T*)
3. Need to be careful with budget - each search page costs 1 unit, each measurement costs 1 unit

## Step B Plan
After finding T*, calculate threshold1 = T* - 32
Need to find all materials that:
- Contain Hg
- Have exactly 4 elements
- Tc >= threshold1

## Step C Plan
Need to induce a rule from measurable materials to predict high/low for unmeasurable ones
Single-factor rules are wrong on several of them

## Step D Plan
Among unmeasurable materials predicted as high, choose one with:
1. Fewest elements
2. If tie, higher expected Tc

## Notes
Will need to be strategic with searches to conserve budget.