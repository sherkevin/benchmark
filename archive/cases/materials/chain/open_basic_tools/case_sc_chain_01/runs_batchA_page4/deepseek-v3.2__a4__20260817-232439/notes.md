# Superconductor Tc Study Notes

## Initial Setup
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc definition: Tc >= 90 K
- Need to complete all four steps (A, B, C, D)

## Step A - Series Peak
Goal: Find homologous doping series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen content. Identify composition with maximum Tc (T*).

Approach:
1. First, measure seed material Tc to establish baseline
2. Search for materials with same element set (O, Ca, Cu, Ba, Hg)
3. Find all series members
4. Measure their Tc values
5. Identify maximum Tc (T*)

## Step B - Filter Family B
Goal: Among materials containing Hg and having exactly 4 elements, report all with Tc >= threshold1 (T* - 32)

## Step C - Extrapolate
Goal: Induce rule from measurable materials, predict high/low for unmeasurable ones

## Step D - Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc)

## Measurements Record - Series Members
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (seed material)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample available (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample available (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

## Step A Analysis
Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have Tc = 127.0 K)

## Unmeasurable Materials Found (for Step C)
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

## Step B Requirements
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0 K

## 4-element Hg-containing materials measurements:
1. O4.047Cu1Ba2Hg1: No sample available (unmeasurable)
2. O4.066Cu1Ba2Hg1: No sample available (unmeasurable)
3. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (qualifies: >= 95.0 K)
4. O4.085Cu1Ba2Hg1: Tc = 92.0 K ✗ (does not qualify: < 95.0 K)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓ (qualifies: >= 95.0 K)
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓ (qualifies: >= 95.0 K)
7. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✗ (does not qualify: < 95.0 K)
8. O4.103Cu1Ba2Hg1: Tc = 92.5 K ✗ (does not qualify: < 95.0 K)
9. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓ (qualifies: >= 95.0 K)
10. O4.160Cu1Ba2Hg1: No sample available (unmeasurable)
11. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓ (qualifies: >= 95.0 K)
12. O4.180Cu1Ba2Hg1: No sample available (unmeasurable)
13. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗ (does not qualify: < 95.0 K)
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ (qualifies: >= 95.0 K)

## Q1 (qualifying materials for Step B):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.096Cu1Ba2Hg1 (95.6 K)
3. O4.097Cu1Ba2Hg1 (95.3 K)
4. O4.150Cu1Ba2Hg1 (97.0 K)
5. O4.162Cu1Ba2Hg1 (97.4 K)
6. O4.220Cu1Ba2Hg1 (97.0 K)

## Key Discovery
O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (Low!) - This is the first Low material found.
Elements: O, Cu, Ba, Hg, Tl (5 elements, Tl instead of Ca)

## Unmeasurable materials found (for Step C):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, Ca series)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, Ca series)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. O4.066Cu1Ba2Hg1 (4 elements)
5. O4.160Cu1Ba2Hg1 (4 elements)
6. O4.180Cu1Ba2Hg1 (4 elements)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6+ elements, contains Tl and Pb)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5+ elements, contains Tl)

## New Measurements:
1. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (High!) - contains Ca and Tl
2. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (Low) - contains Cd and Tl

## Measurable Low materials:
1. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (Low) - contains Hg and Tl
2. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (Low) - contains Cd and Tl

## Measurable High materials with Tl:
1. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (High) - contains Ca and Tl

## Unmeasurable materials found (for Step C):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, Ca series)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, Ca series)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. O4.066Cu1Ba2Hg1 (4 elements)
5. O4.160Cu1Ba2Hg1 (4 elements)
6. O4.180Cu1Ba2Hg1 (4 elements)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6+ elements, contains Tl and Pb)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5+ elements, contains Tl)

## Current Rule Hypothesis:
A material is High if it contains:
1. Hg without Tl, OR
2. Ca (with or without Tl)

Tested on measurable materials:
- All Hg-containing without Tl: High ✓ (21 materials)
- O6Cu1Ba2Hg1Tl1 (Hg with Tl): Low ✓
- O10Ca2Cu3Ba2Tl2 (Ca with Tl): High ✓
- O6Cu1Cd0.250Ba2Tl1.750 (no Hg, no Ca): Low ✓

## Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Contains Ca → High
2. O6.270Ca1Cu2Ba2Hg1: Contains Ca → High
3. O4.047Cu1Ba2Hg1: Contains Hg without Tl → High
4. O4.066Cu1Ba2Hg1: Contains Hg without Tl → High
5. O4.160Cu1Ba2Hg1: Contains Hg without Tl → High
6. O4.180Cu1Ba2Hg1: Contains Hg without Tl → High
7. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ca → High
8. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ca → High

All predicted High.

## Final Decision for Step D:
Among unmeasurable materials predicted High:
- Fewest elements: 4 elements
- 4-element unmeasurable materials: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- Based on oxygen content and nearby Tc measurements, O4.160Cu1Ba2Hg1 likely has highest expected Tc (~97.2 K)
- Final pick: O4.160Cu1Ba2Hg1

## Budget Tracking
Initial budget: 45 units
Used: 42 units (26 measure_tc + 16 search_db pages)
Remaining: 3 units