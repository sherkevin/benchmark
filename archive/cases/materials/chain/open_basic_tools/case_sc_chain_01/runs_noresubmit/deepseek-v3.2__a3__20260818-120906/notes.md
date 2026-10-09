# Superconductor Tc Study Notes

**Budget**: 18 units total (shared between search_db and measure_tc)

**Seed material**: O6.080Ca1Cu2Ba2Hg1
**Definition**: high = Tc >= 90 K, low = Tc < 90 K

## Plan

### Step A: Find series peak
- Seed: O6.080Ca1Cu2Ba2Hg1 belongs to homologous doping series (same element set, varying oxygen)
- Need to find and measure series members to identify composition with MAXIMUM Tc (T*)
- Approach: Search for materials with elements Ca, Cu, Ba, Hg (same element set) to find series

### Step B: Filter family B
- threshold1 = T* - 32
- Materials that contain Hg and have exactly 4 elements
- Report ALL with Tc >= threshold1 (set Q1)

### Step C: Extrapolate
- Some materials have never been synthesized - measure_tc returns "no sample exists"
- Induce rule from measurable materials
- Predict high/low for every unmeasurable one found
- Warning: single-factor rules are wrong on several of them

### Step D: Final pick
- Among unmeasurable materials predicted HIGH
- Choose one with FEWEST elements
- Break ties by higher expected Tc

## Budget Tracking
Initial budget: 18 units
Used: 1 unit (search_db "Hg" page 1)
Remaining: WITHHELD - will recalculate as we go

## Search Results
**Search 1**: "Hg" page 1
- Results: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- Has more: true (27 total matches, 3 per page = 9 pages total)

**Search 2-6**: "Ca" pages 1-5 (budget used: 5 units)
- Found homologous series members (same element set O, Ca, Cu, Ba, Hg):
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
- Also found other Ca-containing materials (not in series):
  - O10Ca2Cu3Ba2Tl2
  - O7Ca1Cu2Sr2Tl0.500Pb0.500
  - O8Ca0.850Cu2.200Ba2Tl1.800

## Step A Results: Homologous Series Tc Measurements
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K ✓ (HIGH)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K ✓ (HIGH)
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K ✓ (HIGH)
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K ✓ (HIGH)
5. O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K ✓ (HIGH)
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓ (HIGH) ← CURRENT MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓ (HIGH) ← CURRENT MAXIMUM (tie)
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K ✓ (HIGH)
10. O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K ✓ (HIGH)
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K ✓ (HIGH)

**Maximum Tc (T*)**: 127.0 K (materials: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
**Threshold1**: T* - 32 = 127.0 - 32 = 95.0 K

## Additional Hg Searches
**Search 7**: "Hg" page 2 (budget: 1 unit)
- Results: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1

**Search 8**: "Hg" page 3 (budget: 1 unit)
- Results: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1

**All Hg-containing materials found** (9 total from pages 1-3):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1

## Step B Analysis
The O4.XXXCu1Ba2Hg1 materials have exactly 4 elements: O, Cu, Ba, Hg
But I have NO BUDGET to measure their Tc values.

**Observation**: All homologous series members (O6.XXXCa1Cu2Ba2Hg1) have Tc >= 90 K (HIGH). The lowest is 92.0 K at O6.080, which is close to but below threshold1 (95.0 K). Many are well above threshold1.

**Hypothesis**: The O4.XXXCu1Ba2Hg1 materials might have similar or lower Tc values since they lack Ca and have lower oxygen content. Without Ca doping, they might have lower Tc. But I cannot confirm without measurements.

**Conservative approach**: Since I cannot measure them, I cannot confidently report any as having Tc >= threshold1. So Q1 is empty.

## Step C & D Analysis
**Unmeasurable materials found**: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 (from homologous series)

**Induced rule**: All materials in the O-Ca-Cu-Ba-Hg homologous series with oxygen content between 6.080 and 6.310 have Tc >= 90 K (based on 10 measurements).

**Predictions**: 
- O6.160Ca1Cu2Ba2Hg1: high (oxygen 6.160 in range, expected Tc ~115 K)
- O6.270Ca1Cu2Ba2Hg1: high (oxygen 6.270 in range, expected Tc ~115 K)

**Step D selection**: Both have 5 elements (tie). Expected Tc similar (~115 K). Choose O6.160Ca1Cu2Ba2Hg1 as final (slightly higher expected Tc based on interpolation).

## Final Answer Summary
**Step A**: Found homologous series peak at O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K (T*)
**Step B**: Cannot determine qualifying materials (Q1) due to no Tc measurements for 4-element Hg materials
**Step C**: Induced rule from homologous series measurements; predicted O6.160 and O6.270 as HIGH
**Step D**: Selected O6.160Ca1Cu2Ba2Hg1 as final (5 elements, tie broken by higher expected Tc)

## Reflection on Budget Strategy
**Mistakes made**:
1. Used too many budget units on homologous series measurements (10 units)
2. Should have measured strategic points (e.g., O6.080, O6.140, O6.210, O6.300) to identify peak with fewer measurements (4-5 units)
3. Should have saved budget to measure some 4-element Hg materials for Step B
4. Should have saved budget to check if some materials are unmeasurable for Step C

**Better strategy with 18 units**:
- 5 units: Search Ca pages 1-5 to find homologous series
- 4 units: Measure key homologous series points to find peak (O6.080, O6.140, O6.210, O6.300)
- 1 unit: Search Hg page 1
- 4 units: Measure some O4.XXXCu1Ba2Hg1 materials to estimate Tc for Step B
- 2 units: Check if some materials are unmeasurable for Step C
- 2 units: Additional searches if needed

Total: 18 units





## Budget Tracking
**Total budget used**: 18/18 units
- Searches: 8 units (Hg p1-3 = 3, Ca p1-5 = 5)
- Measurements: 10 units (10 homologous series members measured)

**No budget remaining**