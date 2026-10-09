# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high = Tc >= 90 K, low = Tc < 90 K
- Measurement budget: 34 measure_tc calls
- Need to complete 4 dependent steps

## Step A: Series Peak
- Seed belongs to homologous doping series (same element set, varying oxygen)
- Need to measure series members in data/measurable.txt
- Find composition with maximum Tc = T*
- Materials in measurable.txt likely form oxygen doping series with O6.080Ca1Cu2Ba2Hg1

## Step B: Filter Family B
- threshold1 = T* - 32
- Among materials that contain Hg AND have exactly 4 elements
- Report ALL whose Tc >= threshold1 = Q1

## Step C: Extrapolate
- Materials in data/unmeasurable.txt have never been synthesized
- Must induce rule separating high vs low Tc using measurable materials
- Rule must involve interaction of multiple factors (not single-factor)
- Predict high/low for every unmeasurable material

## Step D: Final Pick
- Among unmeasurable materials predicted HIGH in step C
- Return one with FEWEST elements
- Break ties by higher expected Tc

## Data Analysis
### Measurable.txt (27 materials):
1-10: O6.xxxCa1Cu2Ba2Hg1 series (10 materials including seed) - likely Step A series
11-21: O4.xxxCu1Ba2Hg1 series (11 materials) - also Hg-containing, 4 elements
22: O6Cu1Ba2Hg1Tl1 (5 elements, contains Hg)
23: O10Ca2Cu3Ba2Tl2 (6 elements, no Hg)
24: O6Cu1Cd0.250Ba2Tl1.750 (6 elements, no Hg)
25: O7Cu3Ba2La1 (5 elements, no Hg)
26: O7Cu3Y0.200Ba2La0.800 (6 elements, no Hg)

### Unmeasurable.txt (7 materials):
1. O6.160Ca1Cu2Ba2Hg1 - continuation of series from Step A
2. O6.270Ca1Cu2Ba2Hg1 - continuation of series from Step A
3. O4.180Cu1Ba2Hg1 - continuation of O4 series
4. O8Ca0.850Cu2.200Ba2Tl1.800 (6 elements, no Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (7 elements, no Hg)
6. O8Cu3Y1Ba1.900La0.100 (6 elements, no Hg)
7. O8Cu4Y1Ba1.870La0.130 (6 elements, no Hg)

## Measurement Strategy
- Total budget: 34 measurements
- Step A: Need to measure 10 materials (O6.xxxCa1Cu2Ba2Hg1 series)
- Step B: Need to check materials with Hg and exactly 4 elements:
  - O4.xxxCu1Ba2Hg1 series (11 materials) 
  - Possibly O6Cu1Ba2Hg1Tl1 (but has 5 elements, not 4)
  - Possibly seed series (but has 5 elements: O, Ca, Cu, Ba, Hg)
- Total measurements needed: 10 (Step A) + 11 (Step B candidates) = 21 < 34 budget
- We have budget to measure all candidates

# Superconductor Tc Study Notes

## Step A Results (O6.xxxCa1Cu2Ba2Hg1 series):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**Maximum Tc (T*)**: 127.0 K
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first occurrence of 127.0 K)
**threshold1**: T* - 32 = 127.0 - 32 = 95.0 K

## Step B Results (O4.xxxCu1Ba2Hg1 series):
1. O4.081Cu1Ba2Hg1: 98.0 K
2. O4.162Cu1Ba2Hg1: 97.4 K
3. O4.150Cu1Ba2Hg1: 97.0 K
4. O4.220Cu1Ba2Hg1: 97.0 K
5. O4.096Cu1Ba2Hg1: 95.6 K
6. O4.097Cu1Ba2Hg1: 95.3 K
7. O4.047Cu1Ba2Hg1: 95.0 K
8. O4.200Cu1Ba2Hg1: 94.2 K
9. O4.101Cu1Ba2Hg1: 93.4 K
10. O4.103Cu1Ba2Hg1: 92.5 K
11. O4.085Cu1Ba2Hg1: 92.0 K

## Step B Results (Q1):
Threshold = 95.0 K
Qualifying materials (Tc >= 95.0 K):
1. O4.081Cu1Ba2Hg1: 98.0 K ✓
2. O4.162Cu1Ba2Hg1: 97.4 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K ✓
4. O4.220Cu1Ba2Hg1: 97.0 K ✓
5. O4.096Cu1Ba2Hg1: 95.6 K ✓
6. O4.097Cu1Ba2Hg1: 95.3 K ✓
7. O4.047Cu1Ba2Hg1: 95.0 K ✓

Non-qualifying (Tc < 95.0 K):
8. O4.200Cu1Ba2Hg1: 94.2 K ✗
9. O4.101Cu1Ba2Hg1: 93.4 K ✗
10. O4.103Cu1Ba2Hg1: 92.5 K ✗
11. O4.085Cu1Ba2Hg1: 92.0 K ✗

**Q1**: 7 materials qualify

## Other Measurable Materials Results:
22. O6Cu1Ba2Hg1Tl1: 0.0 K (non-superconducting)
23. O10Ca2Cu3Ba2Tl2: 118.97 K (high: >= 90 K)
24. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low: < 90 K)
25. O7Cu3Ba2La1: 89.6 K (low: < 90 K)
26. O7Cu3Y0.200Ba2La0.800: 81.0 K (low: < 90 K)

## Measurements Used: 26/34
## Remaining Measurements: 8

## Induced Rule for Step C:
Based on analysis of all 26 measurable materials, I propose the following rule:

**Material has high Tc (>= 90 K) if:**
1. It contains Hg AND does NOT contain Tl
   OR
2. It contains Tl AND has oxygen content >= 10 AND contains Ca

**Material has low Tc (< 90 K) otherwise.**

### Rule validation on measurable materials:
1. O6.xxxCa1Cu2Ba2Hg1 series: Hg ✓, no Tl ✓ → high ✓ (correct: 92.0-127.0 K)
2. O4.xxxCu1Ba2Hg1 series: Hg ✓, no Tl ✓ → high ✓ (correct: 92.0-98.0 K)
3. O10Ca2Cu3Ba2Tl2: Tl ✓, O10 >= 10 ✓, Ca ✓ → high ✓ (correct: 118.97 K)
4. O6Cu1Cd0.250Ba2Tl1.750: Tl ✓, but O6 < 10 → low ✓ (correct: 88.0 K)
5. O6Cu1Ba2Hg1Tl1: Hg ✓ but contains Tl ✗ → low ✓ (correct: 0.0 K)
6. O7Cu3Ba2La1: no Hg, no Tl → low ✓ (correct: 89.6 K)
7. O7Cu3Y0.200Ba2La0.800: no Hg, no Tl → low ✓ (correct: 81.0 K)

**Rule correctly classifies all 26 measurable materials.**

## Step C: Predictions for Unmeasurable Materials
Applying rule to 7 unmeasurable materials:

1. O6.160Ca1Cu2Ba2Hg1: Hg ✓, no Tl ✓ → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: Hg ✓, no Tl ✓ → **HIGH**
3. O4.180Cu1Ba2Hg1: Hg ✓, no Tl ✓ → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: Tl ✓, O8 < 10 → **LOW**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl ✓, O7 < 10 → **LOW**
6. O8Cu3Y1Ba1.900La0.100: No Hg, no Tl → **LOW**
7. O8Cu4Y1Ba1.870La0.130: No Hg, no Tl → **LOW**

**Predictions**: 3 HIGH, 4 LOW

## Step D: Final Pick
Among HIGH predictions:
- O4.180Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

Material with fewest elements: O4.180Cu1Ba2Hg1 (4 elements)
No tie to break (others have 5 elements).

**Final pick**: O4.180Cu1Ba2Hg1

## Summary
- **Step A**: Peak Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1
- **Step B**: threshold1 = 95.0 K, Q1 = 7 materials with Tc >= 95.0 K
- **Step C**: Induced rule involving Hg/Tl interaction, oxygen content, and Ca presence
- **Step D**: O4.180Cu1Ba2Hg1 selected (fewest elements among high predictions)

**Measurements used**: 26/34 (8 remaining unused)
**All tasks completed successfully**