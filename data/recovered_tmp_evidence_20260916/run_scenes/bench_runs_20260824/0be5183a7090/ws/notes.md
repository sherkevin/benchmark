# Superconductor Discovery Task Notes

## Initial Setup
Task: Find all 65 candidate compositions that satisfy:
1. Exactly 4 distinct chemical elements
2. Tc between 90K and 100K inclusive

Budget: 56 units total shared between search_db (1 per page) and measure_tc (1 per measurement)
Starting composition: C1O5Cu1Sr2

## Initial Measurements
Starting composition C1O5Cu1Sr2:
- Element count: 4 (C, O, Cu, Sr)
- Measured Tc: 0K (far below target range 90-100K)
Budget remaining: 56 (used 1 for measurement)

## Observations
1. Not all 4-element compositions have suitable Tc
2. Need to discover more compositions through search
3. Need efficient search strategy to find all 65 candidates within budget

## Search Strategy
Searching for "Cu" yielded 65 total matches (all candidates)
Page 1 results:
1. C1O5Cu1Sr2 (4 elements) - Tc: 0K
2. C1O5K0.100Cu1Sr1.900 (5 elements)
3. Li0.005O4Cu0.995La2 (4 elements) - to measure
4. Li0.050O4Cu0.995La2 (4 elements) - to measure

Page 2 results:
1. O10Cu2Sr2Nb1Ce0.500Nd1.500 (6 elements)
2. O2Cu1Sr0.850Pr0.150 (4 elements) - to measure
3. O2Na0.180Cl2Ca1.820Cu1 (5 elements)
4. O2S2Cu2Zn1Sr2 (5 elements)

## 4-Element Candidates Found So Far
1. C1O5Cu1Sr2 (Tc: 0K)
2. Li0.005O4Cu0.995La2 (Tc: 0K)
3. Li0.050O4Cu0.950La2 (Tc: 0K)
4. O2Cu1Sr0.850Pr0.150 (Tc:連續 39K)
5. O4.033Cu1Ba2Hg1 (to measure)
6. O4.047Cu1Ba2Hg1 (to measure)
7. O4.066Cu1Ba2Hg1 (to measure)
8. O4.081Cu1Ba2Hg1 (to measure)
9. O4.085Cu1Ba2Hg1 (to measure)

## Key Findings
1. Found Ba-Hg-Cu-O family: O4.xxxCu1Ba2Hg1 series
2. Measured values show trend with oxygen content:
   - O4.033Cu1Ba2Hg1: Tc = 48K
   - O4.066Cu1Ba2Hg1: Tc = 88.5K
   - O4.101Cu1Ba2Hg1: Tc = 93.4K (within target range 90-100K!)
   - O4.370Cu1Ba2Hg1: Tc = 37.5K (decreased again)
3. Some compositions cannot be probed directly (probe_unavailable)
4. Need to infer Tc for non-measurable compositions from family trend

## Current Status
Pages searched: 1-6 (of ~17 needed to cover all 65 candidates)
4-element candidates found: Many in Ba-Hg-Cu-O family
Measured 4-element candidates with Tc:
- C1O5Cu1Sr2: 0K
- Li0.005O4Cu0.995La2: 0K
- Li0.050O4Cu0.950La2: 0K
- O2Cu1Sr0.850Pr0.150: 39K
- O4.033Cu1Ba2Hg1: 48K
- O4.066Cu1Ba2Hg1: 88.5K
- O4.101Cu1Ba2Hg1: 93.4K
- O4.370Cu1Ba2Hg1: 37.5K

Non-measurable 4-element candidates:
- O4.047Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1

## Budget Tracking
Budget used so far: 13 (searches) + measurements
Actually: budget_used shows 13 in last search, budget shows 56
Need to track more carefully

## All Candidates Discovered (65 total)
Through searching all 17 pages for "Cu", I've found all 65 candidates. Now need to:
1. Compile list of all 4-element compositions
2. Measure Tc for measurable ones
3. Infer Tc for non-measurable ones
4. Identify compositions with Tc in 90-100K range

## Key Families Identified
1. Ba-Hg-Cu-O family: O4.xxxCu1Ba2Hg1 series
2. Ba-Tl-Cu-O family: O6.056Cu1Ba2Tl2
3. Sr-Bi-Cu-O family: O6.360Cu1Sr2Bi2
4. Sr-La-Cu-O family: O6Cu2Sr1La2
5. Ba-Pr-Cu-O family: O8Cu4Ba2Pr1
6. Ba-Y-Cu-O family: O8Cu4Y1Ba2.200

## Measured Tc Values So Far
- C1O5Cu1Sr2: 0K
- Li0.005O4Cu0.995La2: 0K
- Li0.050O4Cu0.950La2: 0K
- O2Cu1Sr0.850Pr0.150: 39K
- O4.033Cu1Ba2Hg1: 48K
- O4.066Cu1Ba2Hg1: 88.5K
- O4.101Cu1Ba2Hg1: 93.4K (IN RANGE!)
- O4.370Cu1Ba2Hg1: 37.5K

## Non-measurable Compositions Found
- O4.047Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1

## Final Analysis

### Ba-Hg-Cu-O Family Trend
Measured Tc values:
- O4.033: 48K
- O4.066: 88.5K
- O4.101: 93.4K (in range)
- O4.180: 95.16K (in range)
- O4.220: 97.0K (in range)
- O4.370: 37.5K

Trend shows optimal Tc around O4.18-O4.22 range.

### Non-measurable Compositions in Ba-Hg-Cu-O Family
- O4.047: Between 48K and 88.5K → likely ~68K (NOT in range)
- O4.081: Between 88.5K and 93.4K → likely ~91K (IN RANGE)
- O4.085: Between 88.5K and 93.4K → likely ~91.5K (IN RANGE)
- O4.096: Between 88.5K and 93.4K → likely ~91K (IN RANGE)

### 4-Element Compositions with Tc in 90-100K Range
1. O4.101Cu1Ba2Hg1 (93.4K) - measured
2. O4.180Cu1Ba2Hg1 (95.16K) - measured
3. O4.220Cu1Ba2Hg1 (97.0K) - measured
4. O4.081Cu1Ba2Hg1 (~91K) - inferred
5. O4.085Cu1Ba2Hg1 (~91.5K) - inferred
6. O4.096Cu1Ba2Hg1 (~91K) - inferred

### Other 4-Element Compositions Measured (NOT in range)
- C1O5Cu1Sr2: 0K
- Li0.005O4Cu0.995La2: 0K
- Li0.050O4Cu0.950La2: 0K
- O2Cu1Sr0.850Pr0.150: 39K
- O4.033Cu1Ba2Hg1: 48K
- O4.066Cu1Ba2Hg1: 88.5K
- O4.370Cu1Ba2Hg1: 37.5K
- O4Ca0.100Cu1La1.900: 20.5K
- O4Ca0.130Cu1La1.870: 28K
- O4Cu1Ba0.010La1.990: 0K
- O6.056Cu1Ba2Tl2: 40K
- O6.360Cu1Sr2Bi2: 8K
- O6Cu2Sr1La2: 0K
- O8Cu4Ba2Pr1: 0K
- O8Cu4Y1Ba2.200: 80K

### Highest Tc 4-element candidate measured: O4.220Cu1Ba2Hg1 (97.0K)