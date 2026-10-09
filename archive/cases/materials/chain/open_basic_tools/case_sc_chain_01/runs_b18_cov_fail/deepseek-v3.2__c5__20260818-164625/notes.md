# Superconductor Tc Study Notes

## Starting Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total (shared between measure_tc and search_db)
- Each search_db page costs 1 unit, each measure_tc costs 1 unit
- High Tc: >= 90 K, otherwise Low

## Step A: Series Peak
Goal: Find homologous doping series with same element set, varying oxygen. Find composition with maximum Tc = T*.

Plan:
1. First search for Hg-containing materials to understand the series
2. Look for patterns in oxygen content
3. Measure Tc for all series members found

## Budget Tracking
Initial budget: 18 units
Used: 18 units (all budget consumed)
Remaining: 0 units

## Step A: Homologous Series Members - Tc Measurements
Found 12 materials in the O<number>Ca1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba2Hg1 (seed): Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unsynthesized) ✓
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unsynthesized) ✓
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K ✓

Maximum Tc (T*) = 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (tie)
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence)

## Step B: Filter Family B
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Need materials that:
1. Contain Hg
2. Have exactly 4 elements  
3. Have Tc >= 95.0 K

All materials I measured have 5 elements (O, Ca, Cu, Ba, Hg), so none qualify.
Found O<number>Cu1Ba2Hg1 materials with 4 elements (O, Cu, Ba, Hg) but didn't measure them.
With no budget left, cannot measure them.
Q1 might be empty or incomplete.

## Step C: Extrapolate Rule
Measured materials show pattern: Tc >= 90 K (high) when oxygen content >= 6.130
Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
Both have oxygen > 6.130, so predicted high.

Warning says "single-factor rules are wrong on several of them" - maybe oxygen alone insufficient.

## Step D: Final Pick
Among unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
Both have 5 elements.
Need to choose one with FEWEST elements (tie) -> higher expected Tc.
O6.160 has oxygen 6.160, between O6.140 (105.0) and O6.190 (122.0)
O6.270 has oxygen 6.270, between O6.240 (123.0) and O6.300 (100.0)
O6.160 might have Tc ~113.5 (midpoint), O6.270 might have Tc ~111.5
O6.160 has slightly higher expected Tc.

## Final Reflections
- Used all 18 budget units
- Found homologous series of 12 materials
- Measured Tc for 10, 2 had no samples
- Maximum Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1
- Threshold1 = 95.0 K
- No measurements of 4-element Hg materials (O<number>Cu1Ba2Hg1)
- Predictions based on oxygen content rule
- Final answer written to answer.json