# Superconductor Tc Study Notes

## Initial Setup
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: Tc >= 90 K = "high", otherwise "low"
- Tools: measure_tc costs 1 unit, search_db costs 1 unit per page

## Step A - Series Peak
Goal: Find homologous doping series (same element set, varying oxygen) and find composition with maximum Tc (T*).

Seed composition analysis: O6.080Ca1Cu2Ba2Hg1 has elements: O, Ca, Cu, Ba, Hg (5 elements total)
- Oxygen value: 6.080
- Fixed elements: Ca, Cu, Ba, Hg (ratios fixed at Ca1Cu2Ba2Hg1)

Homologous series found (12 materials with Ca1Cu2Ba2Hg1):
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

## Measurement Results for Homologous Series:
1. O6.080Ca1Cu2Ba2Hg1 - Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Not measured (no budget left)

Maximum Tc (T*) found: 127.0 K (at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Assuming O6.310 would be ≤ 100.0 K based on downward trend after O6.300.

## Problems Identified:
1. Used all budget on Step A (4 searches + 10 measurements)
2. Should have been more strategic: Binary search in homologous series to find peak with fewer measurements
3. Should have reserved budget for Step B (searching for Hg + 4-element materials)
4. Should have measured some 4-element Hg materials to get data for rule induction

## Current Status:
- Budget exhausted (18/18)
- Step A completed: T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1
- Step B incomplete: No data on 4-element Hg materials
- Step C incomplete: Limited rule induction from homologous series only
- Step D incomplete: Only 2 unmeasurable materials found (both 5 elements)

## Final Answer Submitted:
Given budget exhaustion and system instruction to submit based on current knowledge:
- Step A: T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1
- Step B: threshold1 = 95.0 K, qualifying set empty (no 4-element Hg materials measured)
- Step C: Rule induced from homologous series pattern only
- Step D: Selected O6.160Ca1Cu2Ba2Hg1 as final (fewest elements among predicted high, tie broken by higher expected Tc based on interpolation - O6.160 closer to peak than O6.270)

## Optimal strategy (for future reference):
1. Search for homologous series (1 search for pattern, 1-2 more pages if needed)
2. Binary search measurements in series (log2(12) ≈ 4 measurements)
3. Search for Hg materials (multiple pages to find 4-element candidates)
4. Measure representative samples for rule induction
5. Use remaining budget for Step B qualification measurements