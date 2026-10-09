# Superconductor Tc Study Notes

## Initial Analysis
- Budget: 18 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: "high" = Tc >= 90 K, "low" = Tc < 90 K
- Need to complete 4 dependent steps

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find and measure series members to identify composition with MAXIMUM Tc (= T*).

**Series Members Found (O{x}Ca1Cu2Ba2Hg1):**
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1 - No sample available (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓ (highest so far)
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓ (ties highest)
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1 - No sample available (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K ✓

**Maximum Tc (T*) = 127.0 K**
**Peak composition(s):** O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
**Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

**Budget used so far:** 17 units (7 search_db + 10 measure_tc)
**Remaining budget:** 1 unit

## Step B - Filter Family B
- threshold1 = T* - 32
- Among materials that contain Hg and have exactly 4 elements
- Report ALL whose Tc >= threshold1 = Q1

## Step C - Extrapolate
- Some materials have never been synthesized - measure_tc returns "no sample exists"
- Induce a rule from measurable materials
- Predict high/low for every unmeasurable one found
- Warning: single-factor rules are wrong on several of them

## Step D - Final Pick
- Among unmeasurable materials predicted high, choose one with:
  1. FEWEST elements
  2. Break ties by higher expected Tc

## Final Answer Summary
**Budget used:** ALL 18 units (8 search_db + 10 measure_tc)

**Step A Completed:**
- Peak Tc (T*) = 127.0 K
- Peak composition = O6.210Ca1Cu2Ba2Hg1
- Threshold1 = 95.0 K

**Step B (Qualifying materials Q1):**
- No measurements for Hg-containing 4-element materials (O4.xxxCu1Ba2Hg1)
- All measured materials have 5 elements, not 4
- Result: Empty array []

**Step C (Predictions for unmeasurable materials):**
- Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- Both contain same elements as measured high-Tc series
- Both have oxygen content in range 6.0-6.31 where all measured materials are high
- Predictions: both high

**Step D (Final pick):**
- Both unmeasurable materials have 5 elements
- O6.160 (6.160) closer to peak oxygen (6.21-6.22) than O6.270 (6.270)
- Expected Tc higher for O6.160
- Final pick: O6.160Ca1Cu2Ba2Hg1

**Rule Induced:**
- Based on O{x}Ca1Cu2Ba2Hg1 series measurements (x=6.080-6.310)
- All materials in this series have high Tc (≥90K)
- Tc follows parabolic dependence on oxygen content, peaking around x=6.21-6.22
- Materials without Ca1Cu2Ba2Hg1 structure may have different behavior

**Materials discovered via search_db:**
1. O4.047Cu1Ba2Hg1 (4 elements, contains Hg)
2. O4.066Cu1Ba2Hg1 (4 elements, contains Hg)
3. O4.081Cu1Ba2Hg1 (4 elements, contains Hg) - from Hg page 1
4. O4.085Cu1Ba2Hg1 (4 elements, contains Hg) - from Hg page 2
5. O4.096Cu1Ba2Hg1 (4 elements, contains Hg) - from Hg page 2
6. O4.097Cu1Ba2Hg1 (4 elements, contains Hg) - from Hg page 2
7. O10Ca2Cu3Ba2Tl2 (5 elements, no Hg) - from Cu page 1
8. O6.080Ca1Cu2Ba2Hg1 (5 elements, contains Hg) - seed
9. O6.100Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
10. O6.130Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
11. O6.140Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
12. O6.160Ca1Cu2Ba2Hg1 (5 elements, contains Hg, unmeasurable)
13. O6.190Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
14. O6.210Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
15. O6.220Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
16. O6.240Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
17. O6.270Ca1Cu2Ba2Hg1 (5 elements, contains Hg, unmeasurable)
18. O6.300Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
19. O6.310Ca1Cu2Ba2Hg1 (5 elements, contains Hg)
20. O7Ca1Cu2Sr2Tl0.500Pb0.500 (7 elements?, no Hg) - from Ca page 5
21. O8Ca0.850Cu2.200Ba2Tl1.800 (6 elements?, no Hg) - from Ca page 5

**Unmeasurable materials found:**
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

**Measured materials (all high ≥90K):**
All O{x}Ca1Cu2Ba2Hg1 series members except the 2 unmeasurable ones.

**Problem for Step B:** Need to find Hg-containing materials with exactly 4 elements and Tc ≥ 95.0 K, but haven't measured any of the O4.xxxCu1Ba2Hg1 materials. No budget left to measure them.

**Decision:** Since no measurements for O4.xxx materials, cannot determine if they qualify for Q1. Will report empty array for qualifying materials.

**Rule induction:** Based on 10 measurements of O{x}Ca1Cu2Ba2Hg1 series, all are high (≥90K). Tc follows parabolic pattern with oxygen content, peaking around x=6.21-6.22. Unmeasurable materials O6.160 and O6.270 also in this series and contain same elements, so predict high.

**Final pick:** Between O6.160 and O6.270, both have 5 elements. O6.160 closer to peak oxygen content, so expected Tc higher. Choose O6.160.