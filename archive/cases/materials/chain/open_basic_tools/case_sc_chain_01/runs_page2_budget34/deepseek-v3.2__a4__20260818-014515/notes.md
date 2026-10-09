# Superconductor Tc Study Notes

## Task Overview
- Budget: 34 units total (shared between `measure_tc` and `search_db`)
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- High Tc: >= 90 K
- Need to complete 4 steps: A, B, C, D

## Step A: Find series peak
The seed belongs to a homologous doping series with same element set, varying oxygen.
Need to find and measure all series members to identify composition with maximum Tc (T*).

## Step B: Filter family B
Find materials containing Hg and having exactly 4 elements.
Threshold1 = T* - 32.
Report all whose Tc >= threshold1 (set Q1).

## Step C: Extrapolate rule
Some materials have never been synthesized - `measure_tc` returns "no sample exists".
Need to induce a rule from measurable materials, then predict high/low for every unmeasurable one found.
Warning: single-factor rules are wrong on several.

## Step D: Final pick
Among unmeasurable materials predicted high, choose one with fewest elements.
Break ties by higher expected Tc.

## Initial Observations
- Seed composition: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements total)
- Format seems to be: ElementSymbol + count (e.g., O6.080, Ca1, Cu2, Ba2, Hg1)
- Oxygen has fractional count (6.080)
- Need to find series with same element set (Ca, Cu, Ba, Hg) but varying oxygen content

## Planning
1. First search for materials with same elements (Ca, Cu, Ba, Hg) to find the series
2. Measure Tc of each series member
3. Find T* (maximum Tc)
4. Calculate threshold1 = T* - 32
5. Search for Hg-containing materials with exactly 4 elements
6. Measure Tc of those materials to find Q1
7. Analyze measurable materials to find rule
8. Predict unmeasurable materials
9. Choose final material

Budget constraint: 34 units total. Need to be efficient with searches and measurements.

## Step A Progress
Found 12 materials in the series with Ca1Cu2Ba2Hg1:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K (measured)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Progress
Found 15 materials with "Cu1Ba2Hg1" pattern (Hg-containing, potentially 4 elements):
1. O4.047Cu1Ba2Hg1 - Unmeasurable
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1 - Unmeasurable
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1 - Unmeasurable
13. O4.200Cu1Ba2Hg1 - Tc = 94.2 K (< 95.0, doesn't qualify)
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K (>= 95.0, qualifies)
15. O6Cu1Ba2Hg1Tl1 - Has 5 elements (O, Cu, Ba, Hg, Tl), doesn't qualify

Measurements:
- O4.047Cu1Ba2Hg1: Unmeasurable
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (qualifies)
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (doesn't qualify)
- O4.180Cu1Ba2Hg1: Unmeasurable
- O4.160Cu1Ba2Hg1: Unmeasurable

## Budget Summary
Total budget: 34 units
Used: 34 units
Remaining: 0 units

## Step C Analysis
Measurable materials (all high Tc >= 90 K):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K
11. O4.200Cu1Ba2Hg1: 94.2 K
12. O4.220Cu1Ba2Hg1: 97.0 K

Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.160Cu1Ba2Hg1
5. O4.180Cu1Ba2Hg1

Patterns observed:
- All measurable materials are high (Tc >= 90 K)
- Ca1Cu2Ba2Hg1 series shows peak Tc at O6.210-6.220 (127.0 K)
- O4.xxxCu1Ba2Hg1 series shows increasing Tc with oxygen (94.2 at O4.200, 97.0 at O4.220)
- Warning: single-factor rules are wrong on several unmeasurable materials

Possible rule: Tc depends on both oxygen content and presence of certain elements. Materials with Ca tend to have higher Tc for given oxygen content.

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: High (between O6.140 at 105K and O6.190 at 122K)
2. O6.270Ca1Cu2Ba2Hg1: High (between O6.240 at 123K and O6.300 at 100K)
3. O4.047Cu1Ba2Hg1: Low (oxygen too low compared to O4.200 at 94.2K)
4. O4.160Cu1Ba2Hg1: Low (oxygen between 4.047 and 4.200, likely < 90K)
5. O4.180Cu1Ba2Hg1: Borderline, could be low (close to O4.200 at 94.2K)

## Final Decisions

### Step A
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- T* = 127.0 K
- Threshold1 = T* - 32 = 95.0 K

### Step B
- Qualifying materials (Q1): Only O4.220Cu1Ba2Hg1 confirmed with Tc = 97.0 K >= 95.0 K
- Other materials might qualify but couldn't measure due to budget constraints

### Step C
- Rule induced: "A material is high if it contains Ca, OR if it has oxygen content >= 4.2 (for materials without Ca)."
- Predictions:
  - O6.160Ca1Cu2Ba2Hg1: Contains Ca → high
  - O6.270Ca1Cu2Ba2Hg1: Contains Ca → high
  - O4.047Cu1Ba2Hg1: No Ca, oxygen 4.047 < 4.2 → low
  - O4.160Cu1Ba2Hg1: No Ca, oxygen 4.160 < 4.2 → low
  - O4.180Cu1Ba2Hg1: No Ca, oxygen 4.180 < 4.2 → low

### Step D
- Unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- Both have 5 elements
- Break tie by higher expected Tc: O6.160 likely higher (closer to peak at O6.210-6.220)
- Final pick: O6.160Ca1Cu2Ba2Hg1

## Budget Reflection
Used all 34 budget units. Could have been more efficient by:
1. Not searching for unrelated materials early on
2. Measuring fewer materials in Step B and predicting others
3. Better planning to reserve budget for critical measurements

However, completed all steps with reasonable assumptions.