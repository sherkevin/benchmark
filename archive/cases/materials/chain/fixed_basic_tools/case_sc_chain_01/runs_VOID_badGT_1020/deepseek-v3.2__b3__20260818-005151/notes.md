# Superconductor Tc Study - Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as >= 90 K
- Measurement budget: 34 measure_tc calls
- Need to work through 4 steps: A, B, C, D

## Step A: Series Peak
- Homologous series: same element set as seed (O, Ca, Cu, Ba, Hg), varying oxygen stoichiometry
- 12 compositions in series (lines 1-12 of measurable.txt)
- Need to measure Tc for all 12, find maximum Tc (T*)

### Series Members to Measure:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: no sample
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: no sample
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

### Step A Results:
- Maximum Tc (T*) = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
- Need to pick one for answer - will use O6.210Ca1Cu2Ba2Hg1 (first occurrence)

### Step B Calculations:
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

### Step B Results:
Compositions that contain Hg and have exactly 4 elements (O4.xxxCu1Ba2Hg1 series):

Measured Tc values:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (>= 95.0 K)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ (>= 95.0 K)
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ (>= 95.0 K)
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ (>= 95.0 K)
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ (>= 95.0 K)
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ (>= 95.0 K)
7. O4.200Cu1Ba2Hg1: 94.2 K ✗ (< 95.0 K)
8. O4.101Cu1Ba2Hg1: 93.4 K ✗ (< 95.0 K)
9. O4.103Cu1Ba2Hg1: 92.5 K ✗ (< 95.0 K)
10. O4.085Cu1Ba2Hg1: 92.0 K ✗ (< 95.0 K)

No sample available (cannot measure):
- O4.180Cu1Ba2Hg1 (also in unmeasurable.txt)
- O4.047Cu1Ba2Hg1 (also in unmeasurable.txt)

Set Q1 (qualifying compositions):
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1

Total measurements used so far: 25 out of 34 budget

## Final Decisions:

### Step A:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc = 127.0 K)
- Peak Tc (T*): 127.0 K
- threshold1: 127.0 - 32 = 95.0 K

### Step B:
- Qualifying set Q1: [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

### Step C: Induced Rule:
**Rule:** A material has high Tc (>= 90 K) if and only if either:
1. It contains mercury (Hg) but does NOT contain thallium (Tl), OR
2. It contains thallium (Tl) AND also contains calcium (Ca).

Otherwise, it has low Tc (< 90 K).

This rule correctly classifies all 25 measurable compositions.

### Step C Predictions for unmeasurable compositions:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (Hg, no Tl)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (Hg, no Tl)
3. O4.180Cu1Ba2Hg1: HIGH (Hg, no Tl)
4. O4.047Cu1Ba2Hg1: HIGH (Hg, no Tl)
5. O4.066Cu1Ba2Hg1: HIGH (Hg, no Tl)
6. O4.160Cu1Ba2Hg1: HIGH (Hg, no Tl)
7. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (Tl + Ca)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (Tl + Ca)
9. O8Cu3Y1Ba1.900La0.100: LOW (no Hg, no Tl)
10. O8Cu4Y1Ba1.870La0.130: LOW (no Hg, no Tl)

### Step D: Final Pick
Among unmeasurable materials predicted HIGH:
- Fewest elements: O4.xxxCu1Ba2Hg1 compositions (4 elements)
- Tie candidates: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1 (all 4 elements)
- Tie-break by higher expected Tc: Choose O4.180Cu1Ba2Hg1 (oxygen content 4.180 similar to measurable O4.162 with 97.4 K and O4.200 with 94.2 K)

Final answer: O4.180Cu1Ba2Hg1

## Step C: Extrapolate Analysis

### Additional Measurements (lines 25-29 of measurable.txt):
1. O6Cu1Ba2Hg1Tl1: 0.0 K (low, contains Hg + Tl)
2. O10Ca2Cu3Ba2Tl2: 118.97 K (high, contains Tl, no Hg)
3. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low, contains Tl + Cd, no Hg)
4. O7Cu3Ba2La1: 89.6 K (low, contains La, no Hg)
5. O7Cu3Y0.200Ba2La0.800: 81.0 K (low, contains Y + La, no Hg)

### Updated Summary:
**All measured compositions containing ONLY Hg (no Tl):**
- All 10 O4.xxxCu1Ba2Hg1 compositions: Tc = 92.0-98.0 K (high)
- All 10 measurable O6.xxxCa1Cu2Ba2Hg1 series compositions: Tc = 92.0-127.0 K (high)

**Compositions containing BOTH Hg AND Tl:**
- O6Cu1Ba2Hg1Tl1: 0.0 K (low)

**Compositions without Hg:**
- O10Ca2Cu3Ba2Tl2 (Tl only): 118.97 K (high)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl + Cd): 88.0 K (low)
- O7Cu3Ba2La1 (La only): 89.6 K (low)
- O7Cu3Y0.200Ba2La0.800 (Y + La): 81.0 K (low)

### Pattern Hypothesis:
1. Compositions containing ONLY Hg (without Tl) have high Tc
2. Compositions containing BOTH Hg AND Tl have low Tc
3. Compositions without Hg: mixed behavior
   - O10Ca2Cu3Ba2Tl2 has high Tc (118.97 K)
   - Others have low Tc

But O10Ca2Cu3Ba2Tl2 has high Tc while O6Cu1Cd0.250Ba2Tl1.750 has low Tc. Both contain Tl but no Hg. Difference: O10Ca2Cu3Ba2Tl2 has Ca and higher Cu count (3 vs 1), while O6Cu1Cd0.250Ba2Tl1.750 has Cd.

Maybe rule involves: "High Tc if contains Hg without Tl, OR contains Tl without Cd or La/Y combinations"?

### Unmeasurable Compositions to Predict:
1. O6.160Ca1Cu2Ba2Hg1 - Hg, no Tl (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 - Hg, no Tl (5 elements)
3. O4.180Cu1Ba2Hg1 - Hg, no Tl (4 elements)
4. O4.047Cu1Ba2Hg1 - Hg, no Tl (4 elements)
5. O4.066Cu1Ba2Hg1 - Hg, no Tl (4 elements)
6. O4.160Cu1Ba2Hg1 - Hg, no Tl (4 elements)
7. O8Ca0.850Cu2.200Ba2Tl1.800 - no Hg, has Tl (5+ elements)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 - no Hg, has Tl + Pb (6+ elements)
9. O8Cu3Y1Ba1.900La0.100 - no Hg, has Y + La (5 elements)
10. O8Cu4Y1Ba1.870La0.130 - no Hg, has Y + La (5 elements)

## Step D: Final Pick
- Among unmeasurable materials predicted high in step C
- Return one with fewest elements
- Tie-break by higher expected Tc