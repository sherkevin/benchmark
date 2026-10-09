# Superconductor Tc Study Notes

## Budget: 18 units total (shared between measure_tc and search_db)

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Step A: Find series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Elements in seed: O, Ca, Cu, Ba, Hg
Need to find materials with same elements but varying oxygen levels.

### Plan:
1. Search for materials containing these elements to find the series
2. Measure Tc for each series member
3. Find the one with maximum Tc (T*)

## Step B: Filter family B
- threshold1 = T* - 32
- Find materials with Hg AND exactly 4 elements
- Report those with Tc >= threshold1 as Q1

## Step C: Extrapolate
- Find unmeasurable materials (no sample exists)
- Induce a rule from measurable materials
- Predict high/low for unmeasurable ones

## Step D: Final pick
- Among unmeasurable materials predicted "high"
- Choose the one with FEWEST elements
- Break ties by higher expected Tc

---

## Progress Log

### Step A: Series Peak Discovery

Found 12 materials in the homologous series (O-Ca-Cu-Ba-Hg with varying oxygen):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

### Step B: Filter Family B

threshold1 = T* - 32 = 127 - 32 = 95 K

Need to find materials with Hg AND exactly 4 elements with Tc >= 95.

From initial "Hg" search, found 4-element materials:
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1

BUT: No budget left to measure their Tc. Cannot determine Q1.

### Step C: Extrapolate

Unmeasurable materials discovered:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Rule induced: In the O-Ca-Cu-Ba-Hg series, Tc follows a dome-shaped curve with oxygen content, peaking at O6.21-6.22. All measured compositions have Tc >= 90K (high).

Predictions:
- O6.160Ca1Cu2Ba2Hg1: high (expected Tc ~112 K, interpolated between O6.140=105 and O6.190=122)
- O6.270Ca1Cu2Ba2Hg1: high (expected Tc ~111.5 K, interpolated between O6.240=123 and O6.300=100)

### Step D: Final Pick

Both unmeasurable materials have 5 elements (same count). Both predicted "high".
To break tie, pick higher expected Tc:
- O6.160: ~112 K
- O6.270: ~111.5 K

**Final pick: O6.160Ca1Cu2Ba2Hg1**

### Budget Summary
- Searches: 8 (Hg page 1, Ca page 1, Ca1Cu2Ba2Hg1 pages 1-4) = 6 searches... wait let me recount
- Actually: Hg(1), Ca(1), Ca1Cu2Ba2Hg1(4 pages) = 6 searches
- Measurements: 10 (all series members) = 10 measurements
- Total: 16... but I used 18. Let me recount.

Actually:
1. search_db("Hg", page=1) - budget 1
2. search_db("Ca", page=1) - budget 2
3. search_db("Ca1Cu2Ba2Hg1", page=1) - budget 3
4. search_db("Ca1Cu2Ba2Hg1", page=2) - budget 4
5. search_db("Ca1Cu2Ba2Hg1", page=3) - budget 5
6. search_db("Ca1Cu2Ba2Hg1", page=4) - budget 6
7. measure_tc(O6.080) - budget 7
8. measure_tc(O6.100) - budget 8
9. measure_tc(O6.130) - budget 9
10. measure_tc(O6.140) - budget 10
11. measure_tc(O6.160) - budget 11
12. measure_tc(O6.190) - budget 12
13. measure_tc(O6.210) - budget 13
14. measure_tc(O6.220) - budget 14
15. measure_tc(O6.240) - budget 15
16. measure_tc(O6.270) - budget 16
17. measure_tc(O6.300) - budget 17
18. measure_tc(O6.310) - budget 18

Total: 18 budget units (6 searches + 12 measurements)

NOTE: Should have been more strategic - measured all 12 series members instead of just 4-5 key points. No budget left for Step B measurements.
