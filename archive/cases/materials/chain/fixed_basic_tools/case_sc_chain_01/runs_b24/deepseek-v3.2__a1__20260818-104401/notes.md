# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 24 measure_tc calls
- Definition: high Tc >= 90 K, otherwise low

## Step A: Series Peak - COMPLETED
Measured all 10 materials in the O_xCa1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba1Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
I need to pick one composition with maximum Tc - I'll pick O6.210Ca1Cu2Ba2Hg1 as the peak.

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B: Filter Family B
Materials that contain Hg and have exactly 4 elements. Need to:
1. Identify all such materials in measurable.txt
2. Measure their Tc values
3. Calculate threshold1 = T* - 32 (where T* = 127.0 K)
4. Report all with Tc >= threshold1 (95.0 K)

From measurable.txt, I need to identify Hg-containing materials with exactly 4 elements:

List of all measurable materials:
1. O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg) - NOT 4 elements
2. O6.100Ca1Cu2Ba2Hg1 (5 elements)
3. O6.130Ca1Cu2Ba2Hg1 (5 elements)
4. O6.140Ca1Cu2Ba2Hg1 (5 elements)
5. O6.190Ca1Cu2Ba2Hg1 (5 elements)
6. O6.210Ca1Cu2Ba2Hg1 (5 elements)
7. O6.220Ca1Cu2Ba2Hg1 (5 elements)
8. O6.240Ca1Cu2Ba2Hg1 (5 elements)
9. O6.300Ca1Cu2Ba2Hg1 (5 elements)
10. O6.310Ca1Cu2Ba2Hg1 (5 elements)
11. O4.081Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
12. O4.162Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
13. O4.150Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
14. O4.220Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
15. O4.096Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
16. O4.097Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
17. O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
18. O4.200Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
19. O4.101Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
20. O4.103Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
21. O4.085Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - CONTAINS Hg, HAS 4 ELEMENTS
22. O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl) - CONTAINS Hg, NOT 4 elements
23. O10Ca2Cu3Ba2Tl2 (5 elements: O, Ca, Cu, Ba, Tl) - NO Hg
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O, Cu, Cd, Ba, Tl) - NO Hg
25. O7Cu3Ba2La1 (4 elements: O, Cu, Ba, La) - NO Hg
26. O7Cu3Y0.200Ba2La0.800 (5 elements: O, Cu, Y, Ba, La) - NO Hg

## Step B Measurements Completed:
All 11 Hg-containing 4-element materials measured:

1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (≥ 95.0 K)
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓ (≥ 95.0 K)
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓ (≥ 95.0 K)
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ (≥ 95.0 K)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓ (≥ 95.0 K)
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓ (≥ 95.0 K)
7. O4.047Cu1Ba2Hg1: Tc = 95.0 K ✓ (≥ 95.0 K) - Exactly at threshold
8. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗ (< 95.0 K)
9. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✗ (< 95.0 K)
10. O4.103Cu1Ba2Hg1: Tc = 92.5 K ✗ (< 95.0 K)
11. O4.085Cu1Ba2Hg1: Tc = 92.0 K ✗ (< 95.0 K)

Qualifying materials (Tc ≥ 95.0 K):
- O4.081Cu1Ba2Hg1 (98.0 K)
- O4.162Cu1Ba2Hg1 (97.4 K)
- O4.150Cu1Ba2Hg1 (97.0 K)
- O4.220Cu1Ba2Hg1 (97.0 K)
- O4.096Cu1Ba2Hg1 (95.6 K)
- O4.097Cu1Ba2Hg1 (95.3 K)
- O4.047Cu1Ba2Hg1 (95.0 K)

Total measurements used: 21 out of 24 budget.
Remaining budget: 3 measurements.

## Step C: Extrapolate
Need to induce a rule using measurable materials that separates high/low Tc based on multiple factors (not single-factor). Then predict high/low for all 7 unmeasurable materials.

## Additional Measurements for Rule Induction (Used remaining 3 measurements):
22. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (HIGH)
23. O7Cu3Ba2La1: Tc = 89.6 K (LOW)
24. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (LOW)

## All Measurements Summary:
High Tc (≥90 K):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1:255127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K
11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O4.150Cu1Ba2Hg1: 97.0 K
14. O4.220Cu1Ba2Hg1: 97.0 K
15. O4.096Cu1Ba2Hg1: 95.6 K
16. O4.097Cu1Ba2Hg1: 95.3 K
17. O4.047Cu1Ba2Hg1: 95.0 K
18. O4.200Cu1Ba2Hg1: 94.2 K (LOW - but still above 90K, actually HIGH)
19. O4.101Cu1Ba2Hg1: 93.4 K
20. O4.103Cu1Ba2Hg1: 92.5 K
21. O4.085Cu1Ba2Hg1: 92.0 K
22. O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc (<90 K):
23. O7Cu3Ba2La1: 89.6 K
24. O7Cu3Y0.200Ba2La0.800: 81.0 K

Wait, I need to check: O4.200Cu1Ba2Hg1 (94.2 K) is actually HIGH (≥90 K). O4.101Cu1Ba2Hg1 (93.4 K) is HIGH. O4.103Cu1Ba2Hg1 (92.5 K) is HIGH. O4.085Cu1Ba2Hg1 (92.0 K) is HIGH.

So ALL Hg-containing materials are HIGH.
All Ca-containing materials (O_xCa1Cu2Ba2Hg1 series) are HIGH.
O10Ca2Cu3Ba2Tl2 has Ca and is HIGH (118.97 K).

Low Tc materials:
- O7Cu3Ba2La1: No Hg, No Ca, has La
- O7Cu3Y0.200Ba2La0.800: No Hg, No Ca, has Y and La

Pattern: Materials with Ca seem to be HIGH. Materials with Hg seem to be HIGH.
But O7Cu3Ba2La1 has no Ca or Hg, has La, and is LOW.
O7Cu3Y0.200Ba2La0.800 has Y and La, no Ca or Hg, and is LOW.

Let me check if O10Ca2Cu3Ba2Tl2 has Ca and Tl, no Hg, and is HIGH.
Maybe the rule involves Ca OR Hg?

But unmeasurable materials include O8Ca0.850Cu2.200Ba2Tl1.800 which has Ca but no Hg. O7Ca1Cu2Sr2Tl0.500Pb0.500 has Ca but no Hg. O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 have both Ca and Hg.

Need to analyze more deeply...

## Step D: Final Pick
Among unmeasurable materials predicted high in Step C, pick one with fewest elements (break ties by higher expected Tc).