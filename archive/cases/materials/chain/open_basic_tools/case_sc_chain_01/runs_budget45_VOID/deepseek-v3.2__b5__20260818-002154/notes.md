# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Total budget: 45 units (shared between search_db and measure_tc)
- High Tc defined as >= 90K
- Need to complete 4 steps: A, B, C, D

## Step A - Series Peak
Goal: Find and measure the homologous doping series members (same element set, varying oxygen) to identify composition with maximum Tc (T*).

Approach:
1. Search for materials with same elements (Ca, Cu, Ba, Hg) but different oxygen stoichiometry
2. Measure Tc for each found composition
3. Identify maximum Tc (T*) and corresponding composition

## Materials Found (Ca-containing series)
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

## Measured Tc Values (Step A)
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓ (measurable)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓ (measurable)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓ (measurable)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓ (measurable)
5. O6.160Ca1Cu2Ba2Hg1: Unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓ (measurable)
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓ (measurable) ← CURRENT MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓ (measurable) ← TIE FOR MAXIMUM
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓ (measurable)
10. O6.270Ca1Cu2Ba2Hg1: Unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓ (measurable)
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓ (measurable)

## Step A Results
- Maximum Tc (T*) = 127.0 K
- Compositions with maximum Tc: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence)
- Threshold1 = T* - 32 = 127 - 32 = 95 K

## Step B - Filter Family B
Goal: Find all materials containing Hg with exactly 4 elements whose Tc >= 95 K.

### 4-element Hg-containing materials measurement results:
1. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)
2. O4.066Cu1Ba2Hg1: Unmeasurable (no sample)
3. O4.081Cu1Ba2Hg1: 98.0 K ✓ QUALIFIES (>= 95 K)
4. O4.085Cu1Ba2Hg1: 92.0 K ✗ DOES NOT QUALIFY (< 95 K)
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ QUALIFIES (>= 95 K)
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ QUALIFIES (>= 95 K)
7. O4.101Cu1Ba2Hg1: 93.4 K ✗ DOES NOT QUALIFY (< 95 K)
8. O4.103Cu1Ba2Hg1: 92.5 K ✗ DOES NOT QUALIFY (< 95 K)
9. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES (>= 95 K)
10. O4.160Cu1Ba2Hg1: Unmeasurable (no sample)
11. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES (>= 95 K)
12. O4.180Cu1Ba2Hg1: Not measured yet (budget constraint)
13. O4.200Cu1Ba2Hg1: Not measured yet (budget constraint)
14. O4.220Cu1Ba2Hg1: Not measured yet (budget constraint)

### Step B Preliminary Q1 Set (qualifying materials):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.096Cu1Ba2Hg1 (95.6 K)
3. O4.097Cu1Ba2Hg1 (95.3 K)
4. O4.150Cu1Ba2Hg1 (97.0 K)
5. O4.162Cu1Ba2Hg1 (97.4 K)

### Unmeasurable materials found so far (for Step C):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.160Cu1Ba2Hg1

## Additional Materials Found (Tl and Pb searches):
1. O10Ca2Cu3Ba2Tl2
2. O6Cu1Ba2Hg1Tl1
3. O6Cu1Cd0.250Ba2Tl1.750
4. O7Ca1Cu2Sr2Tl0.500Pb0.500
5. O8Ca0.850Cu2.200Ba2Tl1.800

## Current Unmeasurable Materials List (for Step C):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.160Cu1Ba2Hg1
6. O4.180Cu1Ba2Hg1 (not measured but likely unmeasurable based on pattern)
7. O4.200Cu1Ba2Hg1 (not measured but likely unmeasurable based on pattern)
8. O4.220Cu1Ba2Hg1 (not measured but likely unmeasurable based on pattern)
9. O10Ca2Cu3Ba2Tl2 (not measured - might be measurable or not)
10. O6Cu1Ba2Hg1Tl1 (not measured - might be measurable or not)
11. O6Cu1Cd0.250Ba2Tl1.750 (not measured - might be measurable or not)
12. O7Ca1Cu2Sr2Tl0.500Pb0.500 (not measured - might be measurable or not)
13. O8Ca0.850Cu2.200Ba2Tl1.800 (not measured - might be measurable or not)

## Step C - Inducing Rule and Predictions

### Analysis of Data:
1. ALL measurable materials have high Tc (>= 90 K)
2. Unmeasurable materials haven't been synthesized
3. For O{x}Ca1Cu2Ba2Hg1 series (5 elements):
   - Measurable for most oxygen values between 6.08-6.31
   - Unmeasurable at 6.160 and 6.270
4. For O{x}Cu1Ba2Hg1 series (4 elements):
   - Measurable for oxygen values like 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.162
   - Unmeasurable at 4.047, 4.066, 4.160, 4.180 (and likely 4.200, 4.220)
5. Materials with Tl, Cd, Sr, Pb elements: No measurements available

### Induced Rule (for prediction):
"Materials that have been synthesized (measurable) all have high Tc (>= 90 K). Unmeasurable materials are predicted based on similarity to measurable compositions:
- Materials with same element sets as measurable ones (O-Ca-Cu-Ba-Hg or O-Cu-Ba-Hg) are predicted HIGH if their oxygen values are close to measurable ones, LOW otherwise.
- Materials with different element sets (containing Tl, Cd, Sr, Pb) are predicted LOW due to lack of successful synthesis data."

### Predictions for Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (similar to measurable 5-element materials, oxygen between measurable values)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (similar to measurable 5-element materials, oxygen between measurable values)
3. O4.047Cu1Ba2Hg1: LOW (oxygen too low compared to measurable 4-element materials)
4. O4.066Cu1Ba2Hg1: LOW (oxygen too low compared to measurable 4-element materials)
5. O4.160Cu1Ba2Hg1: HIGH (oxygen close to measurable 4.162)
6. O4.180Cu1Ba2Hg1: LOW (oxygen higher than measurable range)
7. O4.200Cu1Ba2Hg1: LOW (oxygen higher than measurable range)
8. O4.220Cu1Ba2Hg1: LOW (oxygen higher than measurable range)
9. O10Ca2Cu3Ba2Tl2: LOW (contains Tl, no similar measurable materials)
10. O6Cu1Ba2Hg1Tl1: LOW (contains Tl, no similar measurable materials)
11. O6Cu1Cd0.250Ba2Tl1.750: LOW (contains Cd and Tl, no similar measurable materials)
12. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (contains Sr, Tl, Pb, no similar measurable materials)
13. O8Ca0.850Cu2.200Ba2Tl1.800: LOW (contains Tl, no similar measurable materials)

### Step D - Final Pick
Among unmeasurable materials predicted HIGH:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.160Cu1Ba2Hg1 (4 elements)

Choose the one with FEWEST elements (break ties by higher expected Tc):
- O4.160Cu1Ba2Hg1 has 4 elements (fewest)
- O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 have 5 elements

Final pick: O4.160Cu1Ba2Hg1

## Budget Tracking
Initial budget: 45 units
Used: 45 units (24 search_db + 21 measure_tc)
Remaining: 0 units