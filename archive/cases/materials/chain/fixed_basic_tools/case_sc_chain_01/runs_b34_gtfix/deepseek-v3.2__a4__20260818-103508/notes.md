# Superconductor Tc Study Notes

## Step A: Finding series peak

Seed material: O6.080Ca1Cu2Ba2Hg1
Budget: 34 measure_tc calls

Plan:
1. First examine measurable.txt to see the series members
2. Measure Tc for each composition in the series
3. Find maximum Tc (T*)

Identified series members (lines 1-10 in measurable.txt):
1. O6.080Ca1Cu2Ba2Hg1 - Tc: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc: 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1 - Tc: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - Tc: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - Tc: 127.0 K (MAXIMUM - T*)
7. O6.220Ca1Cu2Ba2Hg1 - Tc: 127.0 K (also maximum)
8. O6.240Ca1Cu2Ba2Hg1 - Tc: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - Tc: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - Tc: 98.0 K

Step A Result:
T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B: Filter family B (contains Hg and has exactly 4 elements)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Candidates from measurable.txt (lines 11-21):
11. O4.081Cu1Ba2Hg1
12. O4.162Cu1Ba2Hg1
13. O4.150Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
15. O4.096Cu1Ba2Hg1
16. O4.097Cu1Ba2Hg1
17. O4.047Cu1Ba2Hg1
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1

Need to measure Tc for each and find those with Tc >= 95.0 K.

Measurements:
1. O4.081Cu1Ba2Hg1 - Tc: 98.0 K ✓ QUALIFIES
2. O4.162Cu1Ba2Hg1 - Tc: 97.4 K ✓ QUALIFIES
3. O4.150Cu1Ba2Hg1 - Tc: 97.0 K ✓ QUALIFIES
4. O4.220Cu1Ba2Hg1 - Tc: 97.0 K ✓ QUALIFIES
5. O4.096Cu1Ba2Hg1 - Tc: 95.6 K ✓ QUALIFIES
6. O4.097Cu1Ba2Hg1 - Tc: 95.3 K ✓ QUALIFIES
7. O4.047Cu1Ba2Hg1 - Tc: 95.0 K ✓ QUALIFIES
8. O4.200Cu1Ba2Hg1 - Tc: 94.2 K ✗ DOES NOT QUALIFY
9. O4.101Cu1Ba2Hg1 - Tc: 93.4 K ✗ DOES NOT QUALIFY
10. O4.103Cu1Ba2Hg1 - Tc: 92.5 K ✗ DOES NOT QUALIFY
11. O4.085Cu1Ba2Hg1 - Tc:我们发现 92.0 K ✗ DOES NOT QUALIFY

Step B Result:
Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]

## Step C: Extrapolate unmeasurable materials

Data collected (all measurable materials measured):
HIGH (Tc ≥ 90 K): All O6.*Ca1Cu2Ba2Hg1, All O4.*Cu1Ba2Hg1, O10Ca2Cu3Ba2Tl2
LOW (Tc < 90 K): O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800

Induced rule: Material is HIGH if it contains Hg (without Tl) OR contains both Ca and Tl. Material is LOW otherwise.

Testing rule against all measured materials:
- All Hg-containing without Tl: HIGH ✓
- O10Ca2Cu3Ba2Tl2 (Ca and Tl): HIGH ✓
- O6Cu1Ba2Hg1Tl1 (Hg with Tl): LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750 (Tl without Ca): LOW ✓
- O7Cu3Ba2La1 (neither Hg nor Ca+Tl): LOW ✓
- O7Cu3Y0.200Ba2La0.800 (neither Hg nor Ca+Tl): LOW ✓

Rule works for all 26 measured materials.

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Contains Hg, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - Contains Hg, no Tl → HIGH
3. O4.180Cu1Ba2Hg1 - Contains Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Ca and Tl → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Ca and Tl → HIGH
6. O8Cu3Y1Ba1.900La0.100 - No Hg, no (Ca and Tl) → LOW
7. O8Cu4Y1Ba1.870La0.130 - No Hg, no (Ca and Tl) → LOW

## Step D: Final pick

Among unmeasurable materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements  
- O4.180Cu1Ba2Hg1: 4 elements (FEWEST)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Final pick: O4.180Cu1Ba2Hg1 (fewest elements, no tie to break)

## Summary
- Measurements used: 26 out of 34 budget
- Peak Tc: 127.0 K (O6.210Ca1Cu2Ba2Hg1)
- Threshold: 95.0 K
- Qualifying set Q1: 7 compositions
- Rule induced: Hg (without Tl) OR (Ca AND Tl) → HIGH
- Final answer: O4.180Cu1Ba2Hg1

## Step B: Filter family B (contains Hg and has exactly 4 elements)

Will calculate threshold1 = T* - 32
Need to identify materials with Hg and exactly 4 elements from measurable set
Measure their Tc and find those with Tc >= threshold1

## Step C: Extrapolate unmeasurable materials

Examine unmeasurable.txt
Induce rule based on measurable materials
Predict high/low for each unmeasurable material

## Step D: Final pick

Among unmeasurable materials predicted high, pick one with fewest elements
Break ties by higher expected Tc