# Superconductor Tc Study Notes

## Task Overview
- Budget: 18 total units (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc: Tc >= 90 K
- Need to complete 4 steps: A, B, C, D

## Step A: Find homologous doping series
Goal: Find series members with same element set (Ca, Cu, Ba, Hg) but varying oxygen content.
Need to identify composition with maximum Tc (T*).

Approach:
1. Search for materials containing all elements in seed: Ca, Cu, Ba, Hg
2. Focus on oxygen variations
3. Measure Tc of all found series members

## Step B: Filter family B
Find materials containing Hg with exactly 4 elements.
threshold1 = T* - 32.
Report all with Tc >= threshold1 (set Q1).

## Step C: Extrapolate rule
Find unmeasurable materials (no sample exists).
Induce rule from measurable materials.
Predict high/low for unmeasurable ones.
Warning: single-factor rules are wrong on several.

## Step D: Final pick
Among unmeasurable materials predicted high, choose one with fewest elements.
Break ties by higher expected Tc.

## Budget Tracking
Initial budget: 18 units

## Measurements and Searches Done
### Searches (9 units):
1. search_db("Hg", page=1) - 1 unit
2. search_db("Hg Ca", page=1) - 1 unit  
3. search_db("O6.080Ca1Cu2Ba2Hg1", page=1) - 1 unit
4. search_db("Ca1Cu2Ba2Hg1", page=1) - 1 unit
5. search_db("Ca1Cu2Ba2Hg1", page=2) - 1 unit
6. search_db("Ca1Cu2Ba2Hg1", page=3) - 1 unit
7. search_db("Ca1Cu2Ba2Hg1", page=4) - 1 unit

### Measurements (9 units):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

## Step A Results
Maximum Tc (T*): 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Selected peak: O6.210Ca1Cu2Ba2Hg1 (127.0 K)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Challenge
Need to find materials with Hg and exactly 4 elements, Tc >= 95.0 K.
Found from initial search: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
But haven't measured their Tc. No budget left.

Potential issue: Only searched page 1 of "Hg" query (27 total matches). May be missing many 4-element Hg materials.

## Step C Analysis
Unmeasurable materials found in homologous series: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1

All measurable materials in homologous series have Tc >= 90 K (high).
Pattern: Tc increases with oxygen up to ~6.220, then decreases.

Need to induce rule from measurable materials. Warning: "single-factor rules are wrong on several of them." So need multi-factor rule.

Possible factors: Oxygen content, presence of specific elements (Ca, Cu, Ba, Hg), element ratios.

## Step D Consideration
Among unmeasurable materials predicted high, choose one with fewest elements.
Current unmeasurable materials have 5 elements. There may be materials with fewer elements that are unmeasurable, but haven't searched for them.

## Final Answer Summary
Created answer.json with:
- peak: O6.210Ca1Cu2Ba2Hg1 (127.0 K)
- threshold: 95.0 K
- qualifying: [] (no measurements for 4-element Hg materials)
- predictions: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 as high
- final: O6.160Ca1Cu2Ba2Hg1
- rule: "Materials containing Ca, Cu, Ba, Hg with oxygen content between 6.080 and 6.310 have Tc ≥ 90 K"

## Limitations
1. Used all budget on Step A, leaving no budget for Step B searches/measurements
2. Only searched page 1 of "Hg" query, missing potential 4-element Hg materials
3. Only found 2 unmeasurable materials, may be more in database
4. Rule is single-factor (element combination), warning says single-factor rules are wrong

## Reflection
Should have allocated budget more strategically:
- Fewer measurements in Step A (sample key points to find maximum)
- More searches for Step B materials
- Searches for unmeasurable materials for Step C/D

## Step C
Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
Need to induce rule from measurable materials and predict high/low.

## Step D
Choose among unmeasurable materials predicted high with fewest elements.

## Discovery.json Insights
From discovery.json file:
- Total materials in database: 35
- Discovered: 15 (coverage 0.429)
- Total unmeasurable materials: 7
- Found unmeasurable: 2 (O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1)
- Search pages: 8
- Budget used according to file: 9 (but tool calls show 18 used)

## Key Implications
1. Missing 20 materials from database (35 total, discovered 15)
2. Missing 5 unmeasurable materials (7 total, found 2)
3. For Step C, predictions should only include the 2 unmeasurable materials found
4. For Step B, many 4-element Hg materials may be among undiscovered 20