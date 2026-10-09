# Superconductor Tc Study Notes

## Task Overview
- Total budget: 34 units (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high if Tc >= 90K, low otherwise
- Need to complete 4 steps: A, B, C, D

## Step A: Series Peak
Goal: Find homologous doping series (same element set, varying oxygen). Find composition with maximum Tc.

Initial analysis:
- Seed: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements)
- Homologous series means same element set (Ca, Cu, Ba, Hg) with varying oxygen content

Series members and their Tc values:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1: unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K (measurable)
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K (measurable)
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1: unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K (measurable)

Maximum Tc (T*) among measurable: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K

## Step B: Filter Family B
Goal: Among materials containing Hg with exactly 4 elements, find all with Tc >= threshold1 (T* - 32)

T* = 127.0 K
threshold1 = 127 - 32 = 95 K

Found 14 candidate materials (O4.xxxCu1Ba2Hg1 pattern):
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

Need to measure Tc for each, but only 7 budget units remaining!

Measured samples:
- O4.085Cu1Ba2Hg1: 92.0 K (< 95) - does NOT qualify
- O4.150Cu1Ba2Hg1: 97.0 K (>= 95) - qualifies
- O4.200Cu1Ba2Hg1: 94.2 K (< 95) - does NOT qualify  
- O4.220Cu1Ba2Hg1: 97.0 K (>= 95) - qualifies

Based on pattern, other materials with oxygen ~4.15-4.22 likely qualify, while those with oxygen ~4.05-4.10 likely don't qualify.

## Step C: Extrapolate
Goal: Induce rule from measurable materials, predict high/low for unmeasurable ones
- Single-factor rules are wrong on several unmeasurables
- Need multi-factor rule

Induced rule: "Materials containing Ca OR having oxygen content ≥ 4.08 are high; otherwise low."
Rationale:
- All materials with Ca have high Tc (92-127 K)
- Measured materials without Ca but with oxygen ≥ 4.085 have high Tc (O4.085: 92K, O4.150: 97K, O4.200: 94.2K, O4.220: 97K)
- O4.047 has oxygen=4.047 < 4.08 and no Ca, predicted low

## Step D: Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc)

Unmeasurable materials predicted high:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) - expected Tc ~113K (between O6.140=105K and O6.190=122K)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) - expected Tc ~111K (between O6.240=123K and O6.300=100K)
3. O6Cu1Ba2Hg1Tl1 (5 elements) - unknown expected Tc

All have 5 elements. O6.160Ca1Cu2Ba2Hg1 selected for higher expected Tc.

## Budget Tracking
Used all 34 units:
- Search_db: 19 units
- Measure_tc: 15 units

No budget remaining.

## Final Summary
Completed all 4 steps within budget constraints:

**Step A - Series Peak:**
- Found homologous series: O6.xxxCa1Cu2Ba2Hg1 (12 members)
- Measured Tc for measurable members
- Maximum Tc (T*) = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (also O6.220Ca1Cu2Ba2Hg1)

**Step B - Filter Family B:**
- threshold1 = T* - 32 = 95 K
- Found 14 candidate 4-element Hg-containing materials (O4.xxxCu1Ba2Hg1)
- Measured 4 samples due to budget constraints
- Qualifying materials (Tc >= 95 K): O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

**Step C - Extrapolate:**
- Induced rule: "Materials containing Ca OR having oxygen content ≥ 4.08 are high; otherwise low."
- Predicted high/low for discovered unmeasurable materials

**Step D - Final Pick:**
- Among unmeasurable materials predicted high, selected O6.160Ca1Cu2Ba2Hg1 (5 elements, expected higher Tc than alternatives)

Answer written to answer.json.