# Notes for Superconductor Candidate Discovery Task
## Task Started: 2026-08-13

## Overview
I need to find the best unexplored superconductor candidate from 10 unmeasurable materials with a budget of 45 `get_property` calls. "Best" = fewest elements among those with Tc >= 90 K (high), ties broken by higher expected Tc.

Also need to compute Q1: materials containing Hg with exactly 4 elements, Tc >= threshold1, where threshold1 = T* - 32, T* = max Tc in homologous series of seed `O6.080Ca1Cu2Ba2Hg1`.

## Initial Observations
Measurable materials (30 total):
- Lines 1-13: `O6.xxxCa1Cu2Ba2Hg1` series (varying oxygen content)
- Lines asdasd14-24: `O4.xxxCu1Ba2Hg1` series (varying oxygen content)
- Lines 25-30: Various other compositions with different elements

Unmeasurable materials (10 total):
1. O6.160Ca1Cu2Ba2Hg1 (same as measurable line 5?)
2. O6.270Ca1Cu2Ba2Hg1 (same as measurable line 10?)
3. O4.180Cu1Ba2Hg1 (same as measurable line 19?)
4. O4.047Cu1Ba2Hg1 (same as measurable line 20?)
5. O4.066Cu1Ba2Hg1 (new)
6. O4.160Cu1Ba2Hg1 (new)
7. O8Ca0.850Cu2.200Ba2Tl1.800 (complex, 5 elements)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (complex, 6 elements)
9. O8Cu3Y1Ba1.900La0.100 (5 elements)
10. O8Cu4Y1Ba1.870La0.130 (5 elements)

Note: Some unmeasurable materials appear identical to measurable ones. Task says unmeasurable materials have never been probed and `get_property` returns "no measurement exists" for their Tc. Need to infer Tc from measurable materials.

## Plan
1. First, understand the seed series `O6.080Ca1Cu2Ba2Hg1` and find T* (max Tc in its homologous series)
2. Measure Tc values across the measurable series to understand trends
3. Induce a rule for Tc prediction based on composition
4. Apply rule to predict Tc for unmeasurable materials
5. Determine best candidate and Q1 set

## Measurements (Budget: 10/45 used)
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: no measurement (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: no measurement (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

## Observations from O6.xxxCa1Cu2Ba2Hg1 series
- Tc appears to increase with oxygen content up to a peak, then decreases
- Highest measured Tc: 127.0 K at O6.210 and O6.220
- Need to check if T* = 127.0 K or if there could be higher values between measurements
- Pattern: 92.0 → 92.5 → 102.0 → 105.0 → (missing) → 122.0 → 127.0 → 127.0 → 123.0 → (missing) → 100.0 → 98.0

## Measurements O4.xxxCu1Ba2Hg1 series (Budget: 17/45 used)
13. O4.081Cu1Ba2Hg1: Tc = 98.0 K
14. O4.162Cu1Ba2Hg1: Tc = 97.4 K
15. O4.150Cu1Ba2Hg1: Tc = 97.0 K
16. O4.220Cu1Ba2Hg1: Tc = 97.0 K
17. O4.096Cu1Ba2Hg1: Tc = 95.6 K
18. O4.097Cu1Ba2Hg1: Tc = 95.3 K
19. O4.180Cu1Ba2Hg1: no measurement (unmeasurable)
20. O4.047Cu1Ba2Hg1: no measurement (unmeasurable)
21. O4.200Cu1Ba2Hg1: Tc = 94.2 K

## Observations from O4.xxxCu1Ba2Hg1 series
- All Tc values are between 94-98 K, all >= 90 K (high)
- Seems like Tc decreases slightly as oxygen content increases?
- O4.081: 98.0 K (highest)
- O4.200: 94.2 K (lowest among measured)
- These materials have exactly 4 elements (O, Cu, Ba, Hg) and contain Hg → candidates for Q1

## Other Measurable Materials (Budget: 25/45 used)
22. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (low) - 5 elements (O, Cu, Ba, Hg, Tl)
23. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (high) - 5 elements (O, Ca, Cu, Ba, Tl)
24. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (low) - 5 elements (O, Cu, Cd, Ba, Tl)
25. O7Cu3Ba2La1: Tc = 89.6 K (low) - 4 elements (O, Cu, Ba, La)
26. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (low) - 5 elements (O, Cu, Y, Ba, La)

## Preliminary Pattern Analysis
- All O4.xxxCu1Ba2Hg1 materials have high Tc (94-98 K)
- O6.xxxCa1Cu2Ba2Hg1 series has varying Tc (92-127 K), with peak around O6.210-O6.220
- Materials with Tl sometimes have high Tc (O10Ca2Cu3Ba2Tl2: 118.97 K) but O6Cu1Ba2Hg1Tl1 has 0.0 K
- Materials without Hg seem to generally have lower Tc
- The task warns: "one-factor rules (e.g. 'contains Hg', 'has 5 elements') are wrong on several of these materials. The behaviour is governed by an interaction."

## Current Deductions
- T* = max Tc in seed series = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1)
- threshold1 = T* - 32 = 127 - 32 = 95 K
- For Q1 (Hg-containing, 4 elements, Tc >= 95 K):
  - Measurable qualifiers: O4.081Cu1Ba2Hg1 (98.0), O4.162Cu1Ba2Hg1 (97.4), O4.150Cu1Ba2Hg1 (97.0), O4.220Cu1Ba2Hg1 (97.0), O4.096Cu1Ba2Hg1 (95.6)
  - Measurable non-qualifiers: O4.097Cu1Ba2Hg1 (95.3), O4.200Cu1Ba2Hg1 (94.2), O4.101Cu1Ba2Hg1 (93.4), O4.103Cu1Ba2Hg1 (92.5), O4.085Cu1Ba2Hg1 (92.0)
  - Unmeasurable O4.xxx need prediction

## Induced Rule
After analyzing all measurable data (25 measurements):

**Rule**: Materials containing mercury (Hg) have high Tc (>= 90 K) unless they also contain thallium (Tl). The single exception is O10Ca2Cu3Ba2Tl2 which has high Tc despite containing Tl without Hg, due to its specific stoichiometry (Ca2Cu3Ba2Tl2).

**Evidence**:
- All O4.xxxCu1Ba2Hg1 (Hg without Tl): high Tc (94-98 K)
- All O6.xxxCa1Cu2Ba2Hg1 (Hg without Tl): high Tc (92-127 K)  
- O6Cu1Ba2Hg1Tl1 (Hg with Tl): low Tc (0 K)
- O10Ca2Cu3Ba2Tl2 (Tl without Hg): high Tc (118.97 K) - exception
- O6Cu1Cd0.250Ba2Tl1.750 (Tl without Hg): low Tc (88 K)
- O7Cu3Ba2La1 (no Hg or Tl): low Tc (89.6 K)
- O7Cu3Y0.200Ba2La0.800 (no Hg or Tl): low Tc (81 K)

**This rule accounts for the warning about one-factor rules**: A simple "contains Hg" rule would miss the Tl interaction and the O10Ca2Cu3Ba2Tl2 exception.

## Predictions for Unmeasurable Materials
Based on the rule:
1. O6.160Ca1Cu2Ba2Hg1: Hg without Tl → high
2. O6.270Ca1Cu2Ba2Hg1: Hg without Tl → high  
3. O4.180Cu1Ba2Hg1: Hg without Tl → high
4. O4.047Cu1Ba2Hg1: Hg without Tl → high
5. O4.066Cu1Ba2Hg1: Hg without Tl → high
6. O4.160Cu1Ba2Hg1: Hg without Tl → high
7. O8Ca0.850Cu2.200Ba2Tl1.800: Tl without Hg, not same stoichiometry as O10Ca2Cu3Ba2Tl2 → low
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl without Hg, different stoichiometry → low
9. O8Cu3Y1Ba1.900La0.100: no Hg or Tl → low
10. O8Cu4Y1Ba1.870La0.130: no Hg or Tl → low

## Q1 Calculation
- T* = max Tc in seed series = 127.0 K (O6.210Ca1Cu2Ba2Hg1)
- threshold1 = T* - 32 = 95.0 K
- Q1 = materials containing Hg with exactly 4 elements, Tc >= 95 K

**Measurable Q1**: O4.081Cu1Ba2Hg1 (98.0), O4.162Cu1Ba2Hg1 (97.4), O4.150Cu1Ba2Hg1 (97.0), O4.220Cu1Ba2Hg1 (97.0), O4.096Cu1Ba2Hg1 (95.6)

**Predicted Unmeasurable Q1**: All O4.xxx unmeasurable materials are predicted high (>= 90 K). For Q1 threshold (>= 95 K):
- O4.047Cu1Ba2Hg1: predicted > 98.0 K (higher than O4.081) → qualifies
- O4.066Cu1Ba2Hg1: predicted between 98.0 and O4.081 → likely qualifies
- O4.160Cu1Ba2Hg1: predicted ~97.4 K (similar to O4.162) → qualifies
- O4.180Cu1Ba2Hg1: predicted between 97.4 and 94.2, likely ~96 K → qualifies

## Best Candidate Selection
"Best" = fewest elements among those with high Tc, ties broken by higher expected Tc.

Materials with fewest elements among high Tc predictions: 4-element O4.xxx materials.

Among these, O4.160Cu1Ba2Hg1 has highest expected Tc (~97.4 K, similar to O4.162). O4.047 might be higher but less certain. O4.160 is chosen as best candidate.

**Final budget used**:的特 28/45 calls.

## Task Completion Summary
- Measured 28 properties to understand patterns
- Induced rule: Hg → high unless Tl also present, with one exception (O10Ca2Cu3Ba2Tl2)
- Determined T* = 127.0 K, threshold1 = 95.0 K
- Identified Q1 qualifying set (5 measurable + 4 predicted unmeasurable)
- Made predictions for all 10 unmeasurable materials
- Selected O4.160Cu1Ba2Hg1 as best candidate (4 elements, predicted high Tc ~97.4 K)

All deliverables completed: notes.md and answer.json written.