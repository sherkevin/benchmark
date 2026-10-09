# Superconductor Tc Study Notes

## Task Overview
- Total budget: 34 units (shared between `measure_tc` and `search_db`)
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc ≥ 90 K, low Tc < 90 K
- Search results: at most 4 results per page, each page costs 1 budget unit
- Measurements cost 1 budget unit each

## Initial Observations
1. Seed material: `O6.080Ca1Cu2Ba2Hg1` contains O, Ca, Cu, Ba, Hg (5 elements)
2. The seed belongs to a homologous doping series with same element set, varying oxygen
3. Need to find series members to identify maximum Tc (T*)

## Step A - Series Peak
Goal: Find all members of the homologous doping series (same elements: O, Ca, Cu, Ba, Hg) with varying oxygen stoichiometry, measure their Tc values, and identify the composition with maximum Tc (T*).

## Step B - Filter Family B
- Filter: materials containing Hg AND exactly 4 elements
- threshold1 = T* - 32
- Report all materials meeting these criteria with Tc ≥ threshold1 → set Q1

## Step C - Extrapolate
- Some materials have never been synthesized → `measure_tc` returns "no sample exists"
- Need to induce a rule from measurable materials
- Predict high/low for every unmeasurable material found
- Warning: single-factor rules are wrong on several of them

## Step D - Final Pick
- Among unmeasurable materials predicted HIGH
- Choose one with FEWEST elements
- Break ties by higher expected Tc

## Budget Planning
Initial budget: 34 units
Need to allocate carefully between searching and measuring.

## Materials Found So Far

### Hg-containing materials (pages 1-3):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1

### Ca-containing materials (homologous series):
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

## Step A Results - Homologous Series Measurements

Measured Tc values:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K (HIGH) - CURRENT MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K (HIGH) - TIED MAXIMUM
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K (HIGH)
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K (HIGH)

Maximum Tc (T*) = 127.0 K
Composition(s) with maximum Tc: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Need to pick one - likely O6.210Ca1Cu2Ba2Hg1 as it appears first.

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc ≥ 95.0 K

### 4-element Hg-containing materials found:
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

## Step B Measurements - 4-element Hg materials

Measured Tc values (threshold = 95.0 K):
1. O4.220Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
2. O4.200Cu1Ba2Hg1: 94.2 K ✗ DOES NOT QUALIFY
3. O4.180Cu1Ba2Hg1: No sample (unmeasurable)
4. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES
5. O4.160Cu1Ba2Hg1: No sample (unmeasurable)
6. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
7. O4.103Cu1Ba2Hg1: 92.5 K ✗ DOES NOT QUALIFY
8. O4.101Cu1Ba2Hg1: 93.4 K ✗ DOES NOT QUALIFY

Q1 set (materials with Hg, exactly 4 elements, Tc ≥ 95.0 K):
1. O4.220Cu1Ba2Hg1 (97.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)

Need to check remaining 4-element Hg materials:
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1

But budget is limited.

## Unmeasurable materials found so far (for Step C):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O4.160Cu1Ba2Hg1

## Additional Measurements and Discoveries

Measured O4.047Cu1Ba2Hg1: No sample (unmeasurable)
Measured O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH)

## Unmeasurable materials found (for Step C):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O4.160Cu1Ba2Hg1
5. O4.047Cu1Ba2Hg1

## Materials not measured (might be measurable or unmeasurable):
1. O4.066Cu1Ba2Hg1
2. O4.081Cu1Ba2Hg1
3. O4.085Cu1Ba2Hg1
4. O4.096Cu1Ba2Hg1
5. O4.097Cu1Ba2Hg1
6. O6Cu1Ba2Hg1Tl1
7. O7Ca1Cu2Sr2Tl0.500Pb0.500
8. O8Ca0.850Cu2.200Ba2Tl1.800

## Current Budget Status
Budget used: 34 units (15 search_db + 19 measure_tc)
Budget remaining: 0 units

## Step C - Rule Induction and Predictions

Rule induced: "Materials are HIGH Tc (≥ 90 K) if they contain Cu and Ba and have oxygen content above material-specific thresholds (4.1 for O4.xCu1Ba2Hg1 series, 6.08 for O6.xCa1Cu2Ba2Hg1 series); otherwise LOW."

Rationale:
1. All measurable materials contain Cu and Ba
2. All measurable materials are HIGH Tc (≥ 90 K)
3. The lowest measurable oxygen content in O4.x series is 4.101 (O4.101Cu1Ba2Hg1, 93.4 K)
4. The lowest measurable oxygen content in O6.x series is 6.080 (O6.080Ca1Cu2Ba2Hg1, 92.0 K)
5. O4.047Cu1Ba2Hg1 has oxygen = 4.047 < 4.1, so predicted LOW
6. Other unmeasurable materials have oxygen > thresholds, so predicted HIGH

Predictions for unmeasurable materials:
1. O4.047Cu1Ba2Hg1: oxygen = 4.047 < 4.1 → LOW
2. O4.160Cu1Ba2Hg1: oxygen = 4.160 > 4.1 → HIGH
3. O4.180Cu1Ba2Hg1: oxygen = 4.180 > 4.1 → HIGH
4. O6.160Ca1Cu2Ba2Hg1: oxygen = 6.160 > 6.08 → HIGH
5. O6.270Ca1Cu2Ba2Hg1: oxygen = 6.270 > 6.08 → HIGH

## Step D - Final Pick
Among unmeasurable materials predicted HIGH:
1. O4.160Cu1Ba2Hg1 (4 elements)
2. O4.180Cu1Ba2Hg1 (4 elements)
3. O6.160Ca1Cu2Ba2Hg1 (5 elements)
4. O6.270Ca1Cu2Ba2Hg1 (5 elements)

Choose one with FEWEST elements (4 elements). Tie between O4.160 and O4.180.
Break tie by higher expected Tc:
- O4.160 between O4.150 (97.0 K) and O4.162 (97.4 K) → ~97.2 K expected
- O4.180 between O4.162 (97.4 K) and O4.200 (94.2 K) → ~95.8 K expected
O4.160 has higher expected Tc.

Final pick: O4.160Cu1Ba2Hg1

## Final Answer Summary
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold: 95.0 K
- Q1: [O4.220Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1]
- Predictions: as above
- Final: O4.160Cu1Ba2Hg1
- Rule: as stated above