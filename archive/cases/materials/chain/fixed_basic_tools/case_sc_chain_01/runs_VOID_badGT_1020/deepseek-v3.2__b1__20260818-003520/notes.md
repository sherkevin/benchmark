# Superconductor Tc Study Notes

## Initial Analysis
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low Tc < 90 K
- Measurement budget: 34 `measure_tc` calls
- Step A: Measure homologous series (same element set, varying oxygen)
- Step B: Filter materials containing Hg with exactly 4 elements, Tc >= threshold1 (T* - 32)
- Step C: Induce rule from measurable materials to predict high/low for unmeasurable materials
- Step D: Among predicted high materials, pick one with fewest elements (tie-break by higher expected Tc)

## Step A Plan
From `data/measurable.txt`, I need to identify the homologous series with the seed material `O6.080Ca1Cu2Ba2Hg1`. Looking at the file, lines 1-12 appear to be the series with varying oxygen content:
1. O6.080Ca1Cu2Ba2Hg1
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

These all have the same element set: O, Ca, Cu, Ba, Hg.

I need to measure Tc for all 12 compositions to find the maximum Tc (T*).

## Step A Measurements (Homologous Series)
Note: Some compositions in measurable.txt are actually unmeasurable (no sample exists).

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

Maximum Tc among measurable compositions: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Measurements (Hg-containing materials with exactly 4 elements)
Candidates: O4.xxxCu1Ba2Hg1 series (lines 13-24 in measurable.txt)
All contain Hg and have exactly 4 elements: O, Cu, Ba, Hg

Measured Tc values:
13. O4.081Cu1Ba2Hg1: 98.0 K (>= 95.0 K) ✓
14. O4.162Cu1Ba2Hg1: 97.4 K (>= 95.0 K) ✓
15. O4.150Cu1Ba2Hg1: 97.0 K (>= 95.0 K) ✓
16. O4.220Cu1Ba2Hg1: 97.0 K (>= 95.0 K) ✓
17. O4.096Cu1Ba2Hg1: 95.6 K (>= 95.0 K) ✓
18. O4.097Cu1Ba2Hg1: 95.3 K (>= 95.0 K) ✓
19. O4.180Cu1Ba2Hg1: No sample (unmeasurable)
20. O4.047Cu1Ba2Hg1: No sample (unmeasurable)
21. O4.200Cu1Ba2Hg1: 94.2 K (< 95.0 K) ✗
22. O4.101Cu1Ba2Hg1: 93.4 K (< 95.0 K) ✗
23. O4.103Cu1Ba2Hg1: 92.5 K (< 95.0 K) ✗
24. O4.085Cu1Ba2Hg1: 92.0 K (< 95.0 K) ✗

Qualifying materials (Q1) - Tc >= 95.0 K:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)

## Additional Measurements (for rule induction)
25. O6Cu1Ba2Hg1Tl1: 0.0 K (low)
26. O10Ca2Cu3Ba2Tl2: 118.97 K (high)
27. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low)
28. O7Cu3Ba2La1: 89.6 K (low)
29. O7Cu3Y0.200Ba2La0.800: 81.0 K (low)

## Step C: Rule Induction
Now I have complete measurement data for all measurable materials. I need to induce a rule that separates high (Tc >= 90 K) from low (Tc < 90 K) Tc materials.

Let me organize all measured data with their high/low classification:

### All Measured Materials (Tc values and classification)
High Tc (>= 90 K):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓
11. O4.081Cu1Ba2Hg1: 98.0 K ✓
12. O4.162Cu1Ba2Hg1: 97.4 K ✓
13. O4.150Cu1Ba2Hg1: 97.0 K ✓
14. O4.220Cu1Ba2Hg1: 97.0 K ✓
15. O4.096Cu1Ba2Hg1: 95.6 K ✓
16. O4.097Cu1Ba2Hg1: 95.3 K ✓
17. O4.200Cu1Ba2Hg1: 94.2 K ✓
18. O4.101Cu1Ba2Hg1: 93.4 K ✓
19. O4.103Cu1Ba2Hg1: 92.5 K ✓
20. O4.085Cu1Ba2Hg1: 92.0 K ✓
21. O10Ca2Cu3Ba2Tl2: 118.97 K ✓

Low Tc (< 90 K):
1. O6Cu1Ba2Hg1Tl1: 0.0 K ✗
2. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K ✗
3. O7Cu3Ba2La1: 89.6 K ✗ (borderline but still low)
4. O7Cu3Y0.200Ba2La0.800: 81.0 K ✗

Total measurements used: 25 out of 34 budget.

## Step C: Induced Rule
Based on analysis of all measured materials, I've induced the following rule:

**Rule**: A material has high Tc (>= 90 K) if it satisfies either:
1. Contains Ca (regardless of other elements), OR
2. Contains Hg AND does NOT contain Tl

Otherwise, it has low Tc (< 90 K).

This rule correctly classifies all 25 measured materials.

## Step C: Predictions for Unmeasurable Materials
Now applying the rule to unmeasurable materials from `data/unmeasurable.txt`:

1. O6.160Ca1Cu2Ba2Hg1: Contains Ca → **High**
2. O6.270Ca1Cu2Ba2Hg1: Contains Ca → **High**
3. O4.180Cu1Ba2Hg1: Contains Hg, no Tl → **High**
4. O4.047Cu1Ba2Hg1: Contains Hg, no Tl → **High**
5. O4.066Cu1Ba2Hg1: Contains Hg, no Tl → **High**
6. O4.160Cu1Ba2Hg1: Contains Hg, no Tl → **High**
7. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ca → **High** (contains both Ca and Tl, but rule says Ca regardless)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ca → **High**
9. O8Cu3Y1Ba1.900La0.100: No Ca, no Hg → **Low**
10. O8Cu4Y1Ba1.870La0.130: No Ca, no Hg → **Low**

## Step D: Final Pick
Among unmeasurable materials predicted **High** in step C:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
3. O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
4. O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
5. O4.066Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
6. O4.160Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb)

Materials with fewest elements (4 elements):
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1

Need to break tie by higher expected Tc. Among these 4, all are from the O4.xxxCu1Ba2Hg1 series. Looking at measured Tc values for similar compositions:
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

The oxygen values for the tie-breaking materials are:
- O4.180 (oxygen = 4.180)
- O4.047 (oxygen = 4.047)
- O4.066 (oxygen = 4.066)
- O4.160 (oxygen = 4.160)

Looking at the trend from measured O4 series: Higher oxygen content seems to correlate with higher Tc in some ranges. For example:
- O4.081: 98.0 K
- O4.162: 97.4 K
- O4.150: 97.0 K
- O4.220: 97.0 K
- O4.200: 94.2 K (lower despite higher oxygen)

The trend isn't perfectly monotonic. Looking at the measured O4 series data:
- O4.081 (4.081): 98.0 K
- O4.162 (4.162): 97.4 K  
- O4.150 (4.150): 97.0 K
- O4.220 (4.220): 97.0 K
- O4.096 (4.096): 95.6 K
- O4.097 (4.097): 95.3 K
- O4.200 (4.200): 94.2 K
- O4.101 (4.101): 93.4 K
- O4.103 (4.103): 92.5 K
- O4.085 (4.085): 92.0 K

Among the tie candidates:
- O4.160 (4.160): Closest to O4.162 (97.4 K) and O4.150 (97.0 K)
- O4.180 (4.180): Closer to O4.200 (94.2 K) which has lower Tc
- O4.047 (4.047): Lower oxygen, likely lower Tc
- O4.066 (4.066): Lower oxygen, likely lower Tc

Therefore, O4.160Cu1Ba2Hg1 is likely to have higher expected Tc based on similarity to measured compositions.

## Final Verification
1. **Measurements used**: 25 out of 34 budget (9 remaining)
2. **Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc = 127.0 K)
3. **Threshold**: 95.0 K (127.0 - 32)
4. **Qualifying materials**: All 6 correctly identified (contain Hg, exactly 4 elements, Tc >= 95.0 K)
5. **Rule**: Correctly classifies all 25 measured materials
6. **Predictions**: All 10 unmeasurable materials classified
7. **Final pick**: O4.160Cu1Ba2Hg1 (4 elements, predicted high, highest expected Tc among tie candidates)

The rule "High Tc (>= 90 K) if material contains Ca OR (contains Hg AND does NOT contain Tl); otherwise low Tc (< 90 K)" is consistent with all data.