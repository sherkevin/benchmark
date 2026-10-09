# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 34 calls to measure_tc
- Definition: high Tc >= 90K, low Tc < 90K

## Step A - Series Peak
Need to measure all compositions in data/measurable.txt and find the one with MAXIMUM Tc.

Measured values:
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample (not synthesized)
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K
- O6.220Ca1Cu2Ba2Hg1: 127.0K
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample (not synthesized)
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K
- O4.096Cu1Ba2Hg1: 95.6K
- O4.097Cu1Ba2Hg1: 95.3K
- O4.180Cu1Ba2Hg1: No sample (not synthesized)
- O4.047Cu1Ba2Hg1: No sample (not synthesized)
- O4.200Cu1Ba2Hg1: 94.2K
- O4.101Cu1Ba2Hg1: 93.4K
- O4.103Cu1Ba2Hg1: 92.5K
- O4.085Cu1Ba2Hg1: 92.0K
- O6Cu1Ba2Hg1Tl1: 0.0K
- O10Ca2Cu3Ba2Tl2: 118.97K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0K
- O7Cu3Ba2La1: 89.6K
- O7Cu3Y0.200Ba2La0.800: 81.0K

The composition with the MAXIMUM Tc is O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 with Tc = 127.0K.
So T* = 127.0K and the peak composition is O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1).

Step B: threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Need to find materials that contain Hg and have exactly 4 elements with Tc >= 95.0K.

Materials containing Hg and having exactly 4 elements from measurable.txt:
1. O6.080Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
2. O6.100Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
3. O6.130Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
4. O6.140Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
5. O6.160Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
6. O6.190Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
7. O6.210Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
8. O6.220Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
9. O6.240Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
10. O6.270Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
11. O6.300Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
12. O6.310Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - doesn't qualify
13. O4.081Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 98.0K >= 95.0K ✓
14. O4.162Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 97.4K >= 95.0K ✓
15. O4.150Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 97.0K >= 95.0K ✓
16. O4.220Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 97.0K >= 95.0K ✓
17. O4.096Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 95.6K >= 95.0K ✓
18. O4.097Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 95.3K >= 95.0K ✓
19. O4.180Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - No sample available
20. O4.047Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - No sample available
21. O4.200Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 94.2K < 95.0K ✗
22. O4.101Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 93.4K < 95.0K ✗
23. O4.103Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 92.5K < 95.0K ✗
24. O4.085Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - qualifies - Tc = 92.0K < 95.0K ✗

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}

Wait, I need to double-check which materials contain Hg and have exactly 4 elements. Looking more carefully:
- O6Cu1Ba2Hg1Tl1 - has 5 elements (O, Cu, Ba, Hg, Tl) - doesn't qualify

Actually, looking again at the compositions:
- O4.081Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.162Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.150Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.220Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.096Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.097Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.180Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.047Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.200Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.101Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.103Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓
- O4.085Cu1Ba2Hg1: Elements are O, Cu, Ba, Hg = 4 elements ✓

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1} - those with Tc >= 95.0K

## Step C - Extrapolate Rule
Materials in unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg)
3. O4.180Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
4. O4.047Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
5. O4.066Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
6. O4.160Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
7. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements (O, Ca, Cu, Ba, Tl)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 - 6 elements (O, Ca, Cu, Sr, Tl, Pb)
9. O8Cu3Y1Ba1.900La0.100 - 5 elements (O, Cu, Y, Ba, La)
10. O8Cu4Y1Ba1.870La0.130 - 5 elements (O, Cu, Y, Ba, La)

Now let me analyze the measurable materials to determine the rule for separating high (>=90K) from low (<90K) Tc:

High Tc materials (>=90K):
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K
- O6.220Ca1Cu2Ba2Hg1: 127.0K
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K
- O4.096Cu1Ba2Hg1: 95.6K
- O4.097Cu1Ba2Hg1: 95.3K
- O4.200Cu1Ba2Hg1: 94.2K
- O4.101Cu1Ba2Hg1: 93.4K
- O4.103Cu1Ba2Hg1: 92.5K
- O4.085Cu1Ba2Hg1: 92.0K
- O10Ca2Cu3Ba2Tl2: 118.97K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0K (Actually low Tc)

Low Tc materials (<90K):
- O6Cu1Ba2Hg1Tl1: 0.0K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0K
- O7Cu3Ba2La1: 89.6K
- O7Cu3Y0.200Ba2La0.800: 81.0K

Looking at the pattern:
- Most materials with Hg have high Tc, except O6Cu1Ba2Hg1Tl1 (0.0K)
- Materials with Tl seem to have mixed results (O6Cu1Ba2Hg1Tl1: 0.0K, O10Ca2Cu3Ba2Tl2: 118.97K, O6Cu1Cd0.250Ba2Tl1.750: 88.0K)
- Materials with La are low Tc (O7Cu3Ba2La1: 89.6K, O7Cu3Y0.200Ba2La0.800: 81.0K)
- Oxygen content seems to matter - for similar compositions, certain oxygen ranges yield higher Tc
- For the Ca-Cu-Ba-Hg-O series, oxygen around 6.21-6.22 gives highest Tc
- For the Cu-Ba-Hg-O series, oxygen around 4.08-4.22 gives high Tc

Wait, let me reconsider. Looking more systematically:

Materials with Hg:
- O6.080Ca1Cu2Ba2Hg1: 92.0K (high) - 5 elements
- O6.100Ca1Cu2Ba2Hg1: 92.5K (high) - 5 elements
- O6.130Ca1Cu2Ba2Hg1: 102.0K (high) - 5 elements
- O6.140Ca1Cu2Ba2Hg1: 105.0K (high) - 5 elements
- O6.190Ca1Cu2Ba2Hg1: 122.0K (high) - 5 elements
- O6.210Ca1Cu2Ba2Hg1: 127.0K (high) - 5 elements
- O6.220Ca1Cu2Ba2Hg1: 127.0K (high) - 5 elements
- O6.240Ca1Cu2Ba2Hg1: 123.0K (high) - 5 elements
- O6.300Ca1Cu2Ba2Hg1: 100.0K (high) - 5 elements
- O6.310Ca1Cu2Ba2Hg1: 98.0K (high) - 5 elements
- O4.081Cu1Ba2Hg1: 98.0K (high) - 4 elements
- O4.162Cu1Ba2Hg1: 97.4K (high) - 4 elements
- O4.150Cu1Ba2Hg1: 97.0K (high) - 4 elements
- O4.220Cu1Ba2Hg1: 97.0K (high) - 4 elements
- O4.096Cu1Ba2Hg1: 95.6K (high) - 4 elements
- O4.097Cu1Ba2Hg1: 95.3K (high) - 4 elements
- O4.200Cu1Ba2Hg1: 94.2K (high) - 4 elements
- O4.101Cu1Ba2Hg1: 93.4K (high) - 4 elements
- O4.103Cu1Ba2Hg1: 92.5K (high) - 4 elements
- O4.085Cu1Ba2Hg1: 92.0K (high) - 4 elements
- O6Cu1Ba2Hg1Tl1: 0.0K (low) - 5 elements (has Tl)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0K (low) - 5 elements (has Cd, Tl)

Non-Hg materials:
- O10Ca2Cu3Ba2Tl2: 118.97K (high) - 5 elements (has Tl)
- O7Cu3Ba2La1: 89.6K (low) - 4 elements (has La)
- O7Cu3Y0.200Ba2La0.800: 81.0K (low) - 5 elements (has Y, La)

Looking at this, I notice that materials with Hg are generally high Tc, except when they also contain Tl. The only Hg-containing material with low Tc is O6Cu1Ba2Hg1Tl1 which also contains Tl.

Let me look for a pattern with oxygen content in Hg-free materials:
- O10Ca2Cu3Ba2Tl2: 118.97K (high) - 10 oxygen atoms
- O6Cu1Cd0.250Ba2Tl1.750: 88.0K (low) - 6 oxygen atoms  
- O7Cu3Ba2La1: 89.6K (low) - 7 oxygen atoms
- O7Cu3Y0.200Ba2La0.800: 81.0K (low) - 7 oxygen atoms

Based on analysis, the rule appears to be: A material is high Tc if:
1. It contains Hg AND doesn't contain Tl, OR
2. It contains Tl but no Hg AND has >=10 oxygen atoms (like O10Ca2Cu3Ba2Tl2)

Let me verify this rule:
- O6Cu1Ba2Hg1Tl1: Has both Hg and Tl -> low Tc (matches rule)
- O6Cu1Cd0.250Ba2Tl1.750: Has Tl but no Hg AND only 6 O -> low Tc (matches rule)
- O10Ca2Cu3Ba2Tl2: Has Tl but no Hg AND 10 O -> high Tc (matches rule)

Other materials with Hg but no Tl are all high Tc, which supports the rule.

Now for predictions on unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Contains Hg, no Tl -> high
2. O6.270Ca1Cu2Ba2Hg1 - Contains Hg, no Tl -> high
3. O4.180Cu1Ba2Hg1 - Contains Hg, no Tl -> high
4. O4.047Cu1Ba2Hg1 - Contains Hg, no Tl -> high
5. O4.066Cu1Ba2Hg1 - Contains Hg, no Tl -> high
6. O4.160Cu1Ba2Hg1 - Contains Hg, no Tl -> high
7. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl but no Hg, 8 O < 10 -> low
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Tl but no Hg, 7 O < 10 -> low
9. O8Cu3Y1Ba1.900La0.100 - No Hg, no Tl, contains Y and La -> likely low (similar to O7Cu3Ba2La1 and O7Cu3Y0.200Ba2La0.800)
10. O8Cu4Y1Ba1.870La0.130 - No Hg, no Tl, contains Y and La -> likely low (similar to O7Cu3Ba2La1 and O7Cu3Y0.200Ba2La0.800)

## Step D - Final Pick
From Step C, the unmeasurable materials predicted as high are:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)

The ones with the fewest elements (4) are: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1

Since they all have the same number of elements, I'll pick based on expected Tc. Looking at the pattern from measurable materials in the O4Cu1Ba2Hg1 series:
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K
- O4.096Cu1Ba2Hg1: 95.6K
- O4.097Cu1Ba2Hg1: 95.3K
- O4.200Cu1Ba2Hg1: 94.2K
- O4.101Cu1Ba2Hg1: 93.4K
- O4.103Cu1Ba2Hg1: 92.5K
- O4.085Cu1Ba2Hg1: 92.0K

The ones closest to O4.081 (which had the highest Tc in this series) would likely have high Tc:
- O4.066Cu1Ba2Hg1 is close to O4.081 -> likely highest Tc among the 4-element options
- O4.160Cu1Ba2Hg1 is similar to O4.162 -> likely high Tc (~97.4K)
- O4.180Cu1Ba2Hg1 would likely have Tc around 97K
- O4.047Cu1Ba2Hg1 would likely have Tc around 95-98K

Based on proximity to the highest Tc composition in the series (O4.081), O4.066Cu1Ba2Hg1 might have the highest expected Tc.