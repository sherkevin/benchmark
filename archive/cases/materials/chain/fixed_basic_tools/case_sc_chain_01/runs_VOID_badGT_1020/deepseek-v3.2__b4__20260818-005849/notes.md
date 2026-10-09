# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc: Tc >= 90 K, otherwise low
- Measurement budget: 34 `measure_tc` calls
- Need to complete 4 dependent steps (A-D)

## Step A: Series Peak Analysis
Need to measure homologous doping series (same element set, varying oxygen) from measurable.txt to find maximum Tc (T*).

From data/measurable.txt, the series appears to be lines 1-12 (oxygen values varying from 6.080 to 6.310):
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

These are 12 measurements needed for Step A.

## Step B: Filter Family B
threshold1 = T* - 32
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= threshold1

From measurable.txt, materials with exactly 4 elements and containing Hg:
Lines 13-24 all have format O[x]Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4 elements)
These are 12 additional measurements needed for Step B.

## Step C: Extrapolation Rule
Need to induce a rule separating high vs low Tc based on measurable materials, then predict for unmeasurable ones.
Unmeasurable.txt has 11 materials.

## Step D: Final Pick
From unmeasurable materials predicted high in Step C, return one with fewest elements (break ties by higher expected Tc).

## Measurement Budget
Step A: 12 measurements
Step B: 12 measurements (assuming all Hg-containing 4-element materials)
Total so far: 24 measurements
Remaining: 10 measurements for testing rule induction

## Step A Measurements (Homologous Series)
Measurements for O[x]Ca1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (in unmeasurable.txt)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (in unmeasurable.txt)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc (T*) so far: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
We need to confirm if O6.220Ca1Cu2Ba2Hg1 is indeed the peak or if higher oxygen content would increase Tc further.

From the pattern:
- Tc increases from 92.0K at O6.080 to 127.0K at O6.210-6.220
- Tc then decreases to 123.0K at O6.240, 100.0K at O6.300, and 98.0K at O6.310
- The peak appears to be around O6.210-6.220

T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 (both 127.0K)
Need to check if O6.220 is higher oxygen than O6.210, so O6.220Ca1Cu2Ba2Hg1 might be the peak.

## Budget Used: 10 measurements (budget remaining: 24)

## Step B Measurements (Hg-containing, 4-element materials)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials measured from measurable.txt lines 13-24 (all O[x]Cu1Ba2Hg1):
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ QUALIFIES (≥95.0)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ QUALIFIES
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ QUALIFIES
7. O4.180Cu1Ba2Hg1: No sample (in unmeasurable.txt)
8. O4.047Cu1Ba2Hg1: No sample (in unmeasurable.txt)
9. O4.200Cu1Ba2Hg1: 94.2 K ✗ DOES NOT QUALIFY (<95.0)
10. O4.101Cu1Ba2Hg1: 93.4 K ✗ DOES NOT QUALIFY
11. O4.103Cu1Ba2Hg1: 92.5 K ✗ DOES NOT QUALIFY
12. O4.085Cu1Ba2Hg1: 92.0 K ✗ DOES NOT QUALIFY

Qualifying set Q1 (materials with Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1

## Budget Used: 20 measurements (budget remaining: 14)

## Additional Measurements for Rule Induction
Remaining measurable materials (lines 25-29):
1. O6Cu1Ba2Hg1Tl1: 0.0 K (very low)
2. O10Ca2Cu3Ba2Tl2: 118.97 K (high)
3. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (low)
4. O7Cu3Ba2La1: 89.6 K (low)
5. O7Cu3Y0.200Ba2La0.800: 81.0 K (low)

## All Measurements Summary
High Tc (≥90 K):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1:125.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K
11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O4.150Cu1Ba2Hg1: 97.0 K
14. O4.220Cu1Ba2Hg1: 97.0 K
15. O4.096Cu1Ba2Hg1: 95.6 K
16. O4.097Cu1Ba2Hg1: 95.3 K
17. O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc (<90 K):
1. O4.200Cu1Ba2Hg1: 94.2 K (NOTE: Actually ≥90K but not high enough for threshold1)
2. O4.101Cu1Ba2Hg1: 93.4 K
3. O4.103Cu1Ba2Hg1: 92.5 K
4. O4.085Cu1Ba2Hg1: 92.0 K
5. O6Cu1Ba2Hg1Tl1: 0.0 K
6. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
7. O7Cu3Ba2La1: 89.6 K
8. O7Cu3Y0.200Ba2La0.800: 81.0 K

Wait, I need to be careful - "high" is defined as Tc >= 90 K. So all the O4.xxxCu1Ba2Hg1 materials are actually high (≥90K), but some didn't meet the threshold1 of 95.0K for Step B.

Actually looking at my measurements:
- O4.200Cu1Ba2Hg1: 94.2 K (≥90K, so high)
- O4.101Cu1Ba2Hg1: 93.4 K (≥90K, so high)
- O4.103Cu1Ba2Hg1: 92.5 K (≥90K, so high)
- O4.085Cu1Ba2Hg1: 92.0 K (≥90K, so high)

So ALL O4.xxxCu1Ba2Hg1 materials I measured are high (≥90K). The O6Cu1Ba2Hg1Tl1 (0.0K) is clearly low.
O6Cu1Cd0.250Ba2Tl1.750 (88.0K) is low.
O7Cu3Ba2La1 (89.6K) is low (just below 90K).
O7Cu3Y0.200Ba2La0.800 (81.0K) is low.
O10Ca2Cu3Ba2Tl2 (118.97K) is high.

## Budget Used: pp25 measurements (budget remaining: 9)

Actually correction: I've used 26 measurements now (re-measured O6Cu1Ba2Hg1Tl1).
Budget used: 26 measurements (budget remaining: 8)

## Rule Induction Analysis

Based on all measurements:

**Clear patterns:**
1. All Hg-containing materials WITHOUT Tl are HIGH (≥90K)
2. The one material with BOTH Hg AND Tl is VERY LOW (0K)
3. Tl-containing materials WITHOUT Hg can be HIGH or LOW:
   - O10Ca2Cu3Ba2Tl2 (Ca, high Cu/O): HIGH (118.97K)
   - O6Cu1Cd0.250Ba2Tl1.750 (Cd, low Cu/O): LOW (88K)
4. Materials without Hg or Tl are LOW (O7Cu3Ba2La1: 89.6K, O7Cu3Y0.200Ba2La0.800: 81K)

**Potential rule factors:**
- Hg presence (positive unless with Tl)
- Tl presence (negative unless with Ca and high Cu/O)
- Cd presence (negative with Tl)
- Pb presence (likely negative with Tl)
- Ca presence (positive with Tl)
- Cu stoichiometry (higher Cu better)
- Oxygen content (higher O better)

**Proposed rule**: Material is HIGH if:
1. It contains Hg AND does NOT contain Tl, OR
2. It contains Tl AND contains Ca AND does NOT contain Cd AND does NOT contain Pb AND has Cu ≥ 2 AND O ≥ 8

Otherwise LOW.

Test against measured data:
- All Hg without Tl materials: HIGH ✓
- O6Cu1Ba2Hg1Tl1 (Hg+Tl): LOW ✓
- O10Ca2Cu3Ba2Tl2 (Tl+Ca, no Cd/Pb, Cu3, O10): HIGH ✓
- O6Cu1Cd0.250Ba2Tl1.750 (Tl+Cd): LOW ✓
- O7Cu3Ba2La1 (no Hg/Tl): LOW ✓
- O7Cu3Y0.200Ba2La0.800 (no Hg/Tl): LOW ✓

Rule passes all tests.

## Predictions for Unmeasurable Materials

1. O6.160Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
3. O4.180Cu1Ba2Hg1: Hg without Tl → HIGH
4. O4.047Cu1Ba2Hg1: Hg without Tl → HIGH
5. O4.066Cu1Ba2Hg1: Hg without Tl → HIGH
6. O4.160Cu1Ba2Hg1: Hg without Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: Tl with Ca (0.85), no Cd/Pb, Cu2.2≥2, O8≥8 → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl with Pb → LOW (Pb violates rule)
9. O8Cu3Y1Ba1.900La0.100: No Hg/Tl → LOW
10. O8Cu4Y1Ba1.870La0.130: No Hg/Tl → LOW

**Step D**: Among HIGH predictions (materials 1-7), find one with fewest elements:
- Materials 1-2: 5 elements (O, Ca, Cu, Ba, Hg)
- Materials 3-6: 4 elements (O, Cu, Ba, Hg)
- Material 7: 5 elements? (O, Ca, Cu, Ba, Tl) Actually 5 elements

So materials 3-6 have fewest elements (4). Among these, need to pick one with higher expected Tc. Since all O4.xxxCu1Ba2Hg1 have similar Tc (92-98K), any will do. Pick O4.180Cu1Ba2Hg1 (highest oxygen? 4.180 vs others).

Actually O4.180Cu1Ba2Hg1 has higher oxygen than O4.047, O4.066, O4.160. Among O4.xxx I measured, higher oxygen tends to give slightly higher Tc (O4.220: 97K, O4.081: 98K). O4.180 might be around 97K.

**Final pick**: O4.180Cu1Ba2Hg1 (4 elements, predicted HIGH).

## Final Decisions for Answer.json
- Peak composition: O6.220Ca1Cu2Ba2Hg1 (127.0K, highest oxygen among 127K peaks)
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K
- Qualifying set Q1: 6 O4.xxxCu1Ba2Hg1 materials with Tc ≥ 95.0K
- Predictions: As per rule above
- Final material: O4.180Cu1Ba2Hg1 (4 elements, predicted HIGH, among 4-element HIGH predictions)

Budget used: 26 measurements (8 remaining unused).