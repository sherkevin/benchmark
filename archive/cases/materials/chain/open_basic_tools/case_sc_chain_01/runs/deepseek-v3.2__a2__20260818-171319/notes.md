# Superconductor Tc Study Notes

## Initial Setup
.### Date: 2026-08-18
### Task Overview:
- Budget: 26 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Goal: Complete Steps A-D as described in TASK.md

### Step A Requirements:
Find homologous doping series (same element set: O, Ca, Cu, Ba, Hg) with varying oxygen content.
Measure all series members to find maximum Tc (T*).

### Initial Search:
- Searched for "Hg" (page 1): Found 3 results, 27 total matches
- Results: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- Budget used: 1/26
- Note: Results show 3 per page (not 4 as mentioned in TASK.md)

### Observations:
1. The seed has format: O6.080Ca1Cu2Ba2Hg1
2. Initial Hg search results show format: O4.047Cu1Ba2Hg1 (missing Ca, different Cu count)
3. Need to find series with exact element set: O, Ca, Cu, Ba, Hg

### Searches Conducted (continued):
- Search "CaCuBaHg" (page 1): No results
  - Budget used: 2/26
- Search "Ca" (pages 1-5): 15 total matches
  - Budget used: 7/26 (5 pages searched)
  - Found homologous series: O[x]Ca1Cu2Ba2Hg1 where x varies

### Homologous Series Members (12 materials):
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

### Budget Status:
- Used: (7/26) - Actually 7 used: 1(Hg) + 1(CaCuBaHg) + 5(Ca pages) = 7
- Remaining: 19
- Measurements needed for Step A: 12 (will cost 12, leaving 7 for other steps)

## Step A Results:
### Tc Measurements for Homologous Series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable) ✗
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable) ✗
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: : 98.0 K ✓

### Step A Conclusion:
- Maximum Tc (T*): 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127.0 K)
- Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

### Budget Status:
- Used: 17/26 (7 searches + 10 measurements)
- Remaining: 9

## Step B Requirements:
Find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= threshold1 (95.0 K)
Report this set as Q1.

## Step B Progress:
### Hg-containing materials search:
- Searched pages 1-7 for "Hg" (27 total matches, 3 per page)
- Found 12 materials with pattern `O[x]Cu1Ba2Hg1` (exactly 4 elements)
- Found 15 materials from homologous series `O[x]Ca1Cu2Ba2Hg1` (5 elements)

### Measurements for 4-element materials:
1. O4.047Cu1Ba2Hg1: 95.0 K ✓ (Tc = threshold1 = 95.0 K)
2. O4.081Cu1Ba2Hg1: 98.0 K ✓ (Tc > 95.0 K)
3. O4.085Cu1Ba2Hg1: 92.0 K ✗ (Tc < 95.0 K)

### Budget Status:
- Used all 26/26 budget
- Cannot measure remaining 9 four-element materials

### Step B Challenge:
Need to report ALL materials with Hg, exactly 4 elements, Tc >= 95.0 K.
Found 2 that qualify based on measurements, but 9 unmeasured.
Without budget, cannot determine if others qualify.

## Step C Requirements:
Induce a rule from measurable materials to predict high/low for unmeasurable materials.
Warning: single-factor rules are wrong on several of them.

### Unmeasurable materials found so far:
1. O6.160Ca1Cu2Ba2Hg1 (from homologous series)
2. O6.270Ca1Cu2Ba2Hg1 (from homologous series)

## Step C Analysis:
### Rule Induction:
- All measurable materials found contain both Hg and Cu
- All have Tc >= 90 K (high)
- Proposed rule: "Materials containing both Hg and Cu have Tc >= 90 K"
- This is a two-factor rule (not single-factor)

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Contains Hg and Cu → predicted high
2. O6.270Ca1Cu2Ba2Hg1: Contains Hg and Cu → predicted high

## Step D Selection:
### Among unmeasurable materials predicted high:
- Both have 5 elements (tie)
- Need to break tie by higher expected Tc
- Based on oxygen-Tc trend in homologous series:
  - O6.160 is between O6.140 (105 K) and O6.190 (122 K)
  - O6.270 is between O6.240 (123 K) and O6.300 (100 K)
  - O6.160 likely has higher Tc (closer to peak)
- Final selection: O6.160Ca1Cu2Ba2Hg1

## Final Answer Summary:
### Step A:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold1 = 95.0 K

### Step B:
- Qualifying materials (Hg-containing, exactly 4 elements, Tc >= 95.0 K):
  1. O4.047Cu1Ba2Hg1 (95.0 K)
  2. O4.081Cu1Ba2Hg1 (98.0 K)

### Step C:
- Rule: "Materials containing both Hg and Cu have Tc >= 90 K"
- Predictions:
  - O6.160Ca1Cu2Ba2Hg1: high
  - O6.270Ca1Cu2Ba2Hg1: high

### Step D:
- Final pick: O6.160Ca1Cu2Ba2Hg1

### Budget:
- Used all 26/26 budget
- Could not measure all Hg-containing 4-element materials
- Could not search for additional unmeasurable materials

### Limitations:
- Answer based on available data within budget constraints
- Assumes other Hg-containing 4-element materials either don't qualify or follow similar pattern