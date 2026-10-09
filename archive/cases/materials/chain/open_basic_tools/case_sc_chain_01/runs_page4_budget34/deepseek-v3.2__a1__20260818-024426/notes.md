# Superconductor Tc Study Notes

## Starting Information
-Seed material: O6.080Ca1Cu2Ba2Hg1
-Total budget: 34 units (shared between search_db and measure_tc)
-Each search_db page costs 1 unit, each measure_tc costs 1 unit

## Step A: Series Peak
Need to find homologous doping series with same element set (Ca, Cu, Ba, Hg) but varying oxygen content.

Plan:
1. Search for Hg-containing materials to find similar compositions
2. Look for patterns in oxygen content variation
3. Measure Tc of series members to find maximum Tc (T*)

## Budget Tracking
Starting budget: 34 units
Budget used: 4 units (search_db pages: Hg1, Hg2, Hg3, Ca1)
Budget remaining: 30 units

## Search Results
Page 1 (Hg search):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1  
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1

Page 2 (Hg search):
1. O4.096Cu1Ba2Hg1
2. O4.097Cu1Ba2Hg1
3. O4.101Cu1Ba2Hg1
4. O4.103Cu1Ba2Hg1

Page 3 (Hg search):
1. O4.150Cu1Ba2Hg1
2. O4.160Cu1Ba2Hg1
3. O4.162Cu1Ba2Hg1
4. O4.180Cu1Ba2Hg1

These all have pattern: O[number]Cu1Ba2Hg1 (3 elements besides oxygen: Cu, Ba, Hg)

## Complete Ca-containing Series (O[x]Ca1Cu2Ba2Hg1):
From search results:
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

Other Ca-containing materials (not in series):
1. O10Ca2Cu3Ba2Tl2
2. O7Ca1Cu2Sr2Tl0.500Pb0.500
3. O8Ca0.850Cu2.200Ba2Tl1.800

## Step A Results (Tc measurements for O[x]Ca1Cu2Ba2Hg1 series):
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (high)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (high)
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (high)
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (high)
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (high)
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (high) ← CURRENT MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (high) ← TIE FOR MAXIMUM
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (high)
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (high)
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (high)

Maximum Tc (T*) = 127.0 K
Peak composition(s): O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
Need to check task requirements: "the composition with MAXIMUM Tc" - if tie, need to decide.

## Budget Tracking
Budget used: 21 units (11 searches + 10 measurements)
Budget remaining: 13 units

## Step B: Filter family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find materials that:
1. Contain Hg
2. Have exactly 4 elements total (including oxygen)
3. Have Tc >= 95.0 K

Candidate materials (O[x]Cu1Ba2Hg1 pattern, 4 elements total):
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

Need to measure Tc of these 14 materials. Budget: 13 units remaining.
Some may be unmeasurable (no sample).

## Current Status Update
Budget used: 24 units (11 searches + 13 measurements)
Budget remaining: 10 units

Measurements completed:
1. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)
2. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥95.0, QUALIFIES)
3. O4.200Cu1Ba2Hg1: Tc = 94.2 K (<95.0, doesn't qualify)
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥95.0, QUALIFIES)
5. O4.180Cu1Ba2Hg1: Unmeasurable (no sample)

Qualifying materials for Q1 so far:
1. O4.150Cu1Ba2Hg1 (Tc=97.0)
2. O4.220Cu1Ba2Hg1 (Tc=97.0)

Remaining to measure (9 materials):
1. O4.066Cu1Ba2Hg1  
2. O4.081Cu1Ba2Hg1
3. O4.085Cu1Ba2Hg1
4. O4.096Cu1Ba2Hg1
5. O4.097Cu1Ba2Hg1
6. O4.101Cu1Ba2Hg1
7. O4.103Cu1Ba2Hg1
8. O4.160Cu1Ba2Hg1
9. O4.162Cu1Ba2Hg1

## Step C Considerations
Need to induce rule from measurable materials to predict high/low for unmeasurable ones.
Unmeasurable materials found so far:
1. O6.160Ca1Cu2Ba2Hg1 (from series, oxygen=6.160)
2. O6.270Ca1Cu2Ba2Hg1 (from series, oxygen=6.270)
3. O4.047Cu1Ba2Hg1 (oxygen=4.047)
4. O4.180Cu1Ba2Hg1 (oxygen=4.180)

Rule must be multi-factor (single-factor rules are wrong on several).

## Updated Measurements for Step B:
Completed measurements:
1. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)
2. O4.066Cu1Ba2Hg1: Unmeasurable (no sample)
3. O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥95.0, QUALIFIES)
4. O4.085Cu1Ba2Hg1: Tc = 92.0 K (<95.0, doesn't qualify)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K (≥95.0, QUALIFIES)
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K (≥95.0, QUALIFIES)
7. O4.101Cu1Ba2Hg1: Not measured yet
8. O4.103Cu1Ba2Hg1: Not measured yet
9. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥95.0, QUALIFIES)
10. O4.160Cu1Ba2Hg1: Unmeasurable (no sample)
11. O4.162Cu1Ba2Hg1: Tc = 97.4 K (≥95.0, QUALIFIES)
12. O4.180Cu1Ba2Hg1: Unmeasurable (no sample)
13. O4.200Cu1Ba2Hg1: Tc = 94.2 K (<95.0, doesn't qualify)
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥95.0, QUALIFIES)

Qualifying materials for Q1 (Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0)
2. O4.096Cu1Ba2Hg1 (95.6)
3. O4.097Cu1Ba2Hg1 (95.3)
4. O4.150Cu1Ba2Hg1 (97.0)
5. O4.162Cu1Ba2Hg1 (97.4)
6. O4.220Cu1Ba2Hg1 (97.0)

## Budget Tracking
Budget used: 29 units
Budget remaining: 5 units

Remaining to measure (2 materials):
1. O4.101Cu1Ba2Hg1
2. O4.103Cu1Ba2Hg1

## Step C Analysis
Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 (oxygen=6.160, has Ca)
2. O6.270Ca1Cu2Ba2Hg1 (oxygen=6.270, has Ca)
3. O4.047Cu1Ba2Hg1 (oxygen=4.047, no Ca)
4. O4.066Cu1Ba2Hg1 (oxygen=4.066, no Ca)
5. O4.160Cu1Ba2Hg1 (oxygen=4.160, no Ca)
6. O4.180Cu1Ba2Hg1 (oxygen=4.180, no Ca)

All measurable materials I've found are "high" (Tc ≥ 90 K).
Need multi-factor rule to predict unmeasurable materials.

## Final Summary
Step A: Peak Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220Ca1Cu2Ba2Hg1)
Step B: threshold1 = 95.0 K, Q1 = 6 materials with Tc ≥ 95.0 K
Step C: Rule: Materials with both Hg AND Tl are low; others are high.
Step D: Among unmeasurable high materials, O4.180Cu1Ba2Hg1 has fewest elements (4) and highest expected Tc (based on oxygen trend).

Budget: Used all 34 units.

Rule justification: Only low material found is O6Cu1Ba2Hg1Tl1 (Tc=0.0), which contains both Hg and Tl. All other materials (with Hg only, Tl only, or neither) are high.