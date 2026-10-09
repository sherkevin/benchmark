# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: Tc >= 90 K = "high", Tc < 90 K = "low"
- Measurement budget: 14 `measure_tc` calls

## Step A - Find Maximum Tc in Series
- Series members are in `data/measurable.txt`
- Need to find composition with maximum Tc (T*)

## Step B - Filter Family B
- Materials containing Hg and having exactly 4 elements
- Threshold = T* - 32
- Report all with Tc >= threshold1 (set Q1)

## Step C - Induce Rule and Predict Unmeasurables
- Materials in `data/unmeasurable.txt` have no physical samples
- Must induce rule from measurable materials that separates high vs low Tc
- Rule involves interaction of multiple factors (not single-factor)

## Step D - Final Pick
- Among unmeasurables predicted HIGH in step C
- Pick one with FEWEST elements
- Tie-break by higher expected Tc

## Budget Management
Total budget: 14 measurements
Step A: measured all series members (10 measurements used)
Step B: need to measure materials with Hg and exactly 4 elements (11 materials, but only 4 measurements left)
Step C: no measurements allowed
Step D: no additional measurements needed

## Step B Analysis
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials in measurable.txt with Hg and exactly 4 elements (lines 11-21):
11. O4.081Cu1Ba2Hg1 - NOT MEASURED
12. O4.162Cu1Ba2Hg1 - NOT MEASURED
13. O4.150Cu1Ba2Hg1 - Tc = 97.0 K (>= 95.0 K) - QUALIFIES
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K (>= 95.0 K) - QUALIFIES
15. O4.096Cu1Ba2Hg1 - NOT MEASURED
16. O4.097Cu1Ba2Hg1 - NOT MEASURED
17. O4.047Cu1Ba2Hg1 - Tc = 95.0 K (>= 95.0 K) - QUALIFIES
18. O4.200Cu1Ba2Hg1 - NOT MEASURED
19. O4.101Cu1Ba2Hg1 - NOT MEASURED
20. O4.103Cu1Ba2Hg1 - NOT MEASURED
21. O4.085Cu1Ba2Hg1 - Tc = 92.0 K (< 95.0 K) - DOES NOT QUALIFY

Measurements made (4 total for Step B):
1. O4.047Cu1Ba2Hg1 - 95.0 K (qualifies)
2. O4.150Cu1Ba2Hg1 - 97.0 K (qualifies)
3. O4.220Cu1Ba2Hg1 - 97.0 K (qualifies)
4. O4.085Cu1Ba2Hg1 - 92.0 K (does not qualify)

All 14 measurements used. Cannot measure remaining materials.

Problem: Need to report ALL materials with Tc >= threshold1, but only measured 4 out of 11.

## Step C - Induce Rule (Final)

After analyzing all data, I propose this rule:
"Materials with integer stoichiometry for all non-oxygen elements are high Tc (>= 90 K); materials with fractional stoichiometry for any non-oxygen element are low Tc (< 90 K)."

Rationale:
- All measured materials (14 total) have integer stoichiometry for non-oxygen elements (Ca1, Cu1/2, Ba2, Hg1) and all are high Tc (>= 90 K)
- Unmeasurable materials #1-3 have integer stoichiometry for non-oxygen elements and are predicted high
- Unmeasurable materials #4-7 have fractional stoichiometry for non-oxygen elements (e.g., Ca0.850, Cu2.200, Tl1.800, Ba1.900, La0.100, etc.) and are predicted low

This rule involves multiple factors (stoichiometry integrity across multiple elements) rather than a single factor like "contains Hg".

## Step D - Final Pick
Among unmeasurables predicted HIGH (#1-3):
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements

Pick O4.180Cu1Ba2Hg1 (fewest elements, 4).

## Final Answer Summary
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold1: 95.0 K
- Qualifying materials (Q1): O4.047Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1 (only these 3 were measured and found to have Tc >= 95.0 K)
- Predictions: High for #1-3, low for #4-7 based on integer stoichiometry rule
- Final pick: O4.180Cu1Ba2Hg1

## Limitations
- Only measured 4 out of 11 O4.xxx materials for Step B; others' Tc values unknown
- No low Tc materials measured; rule induction based on pattern analysis rather than direct low Tc examples
- Budget allocation might not have been optimal (used all 10 measurements for Step A series, leaving only 4 for Step B)

## Step A Measurements
Need to measure series members 1-10 from measurable.txt:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Maximum Tc (T*) = 127.0 K
Composition with maximum Tc = O6.210Ca1Cu2Ba2Hg1 (tie with O6.220Ca1Cu2Ba2Hg1, both 127.0 K)
Will use O6.210Ca1Cu2Ba2Hg1 as peak composition (first occurrence)

Budget used so far: 10 measurements
Remaining budget: 4 measurements