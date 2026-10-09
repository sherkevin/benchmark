# Superconductor Tc Study Notes

## Task Overview
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high = Tc >= 90 K, low = Tc < 90 K

## Step A: Find series peak
Need to find homologous doping series with same element set, varying oxygen.
Seed: O6.080Ca1Cu2Ba2Hg1 (elements: O, Ca, Cu, Ba, Hg)

## Step B: Filter family B
Find materials containing Hg with exactly 4 elements, Tc >= threshold1
threshold1 = T* - 32 (where T* is max Tc from Step A)

## Step C: Extrapolate
Induce rule from measurable materials, predict high/low for unmeasurable ones
Warning: single-factor rules are wrong on several of them

## Step D: Final pick
Among unmeasurable materials predicted high, choose one with fewest elements
Break ties by higher expected Tc

## Step A Results
Found 12 materials in homologous series (Ca1Cu2Ba2Hg1 with varying O):
1. O6.080Ca1Cu2Ba2Hg1 - Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K (measurable)

Maximum Tc (T*) = 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B Parameters
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find materials containing Hg with exactly 4 elements, Tc >= 95.0 K

## Step C: Unmeasurable materials found so far
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

## Step B: Materials with exactly 4 elements (containing Hg)
Found 14 O4.xxxCu1Ba2Hg1 materials:
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

Step B Measurements (partial due to budget constraints):
1. O4.047Cu1Ba2Hg1 - Unmeasurable (no sample)
2. O4.101Cu1Ba2Hg1 - Tc = 93.4 K (below threshold 95.0 K)
3. O4.150Cu1Ba2Hg1 - Tc = 97.0 K (above threshold 95.0 K) ✓
4. O4.160Cu1Ba2Hg1 - Unmeasurable (no sample)
5. O4.180Cu1Ba2Hg1 - Unmeasurable (no sample)
6. O4.220Cu1Ba2Hg1 - Tc = 97.0 K (above threshold 95.0 K) ✓

Based on pattern: Tc seems to increase with oxygen content. O4.101 has Tc=93.4, O4.150 has Tc=97.0, O4.220 has Tc=97.0.
Threshold: 95.0 K

Likely qualifying materials (Tc >= 95.0 K): O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, and possibly others with oxygen > 4.150

## Step C: Unmeasurable materials found
From Step A:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

From Step B and O4 search:
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.081Cu1Ba2Hg1
6. O4.085Cu1Ba2Hg1
7. O4.160Cu1Ba2Hg1
8. O4.180Cu1Ba2Hg1

Also: O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.162Cu1Ba2Hg1 (from earlier searches, not measured)

## Induced Rule
From measurable materials:
- All Ca-series materials (O6.xxxCa1Cu2Ba2Hg1) are high (Tc >= 90 K)
- All CuBaHg-series materials with oxygen >= 4.101 are high
- O4.101 has Tc=93.4 K (just above threshold)

Pattern: Tc increases with oxygen content in both series.
For CuBaHg series: Low oxygen (< 4.101) likely gives Tc < 90 K (low)
For Ca series: All are high regardless of oxygen (92-127 K range)

But warning: "single-factor rules are wrong on several of them"
Possible additional factor: Number of elements? All Ca-series have 5 elements, CuBaHg have 4.
Or presence of Ca vs Cu stoichiometry?

## Predictions
Based on oxygen content:
1. O6.160Ca1Cu2Ba2Hg1 - High (in Ca series, all high)
2. O6.270Ca1Cu2Ba2Hg1 - High (in Ca series, all high)
3. O4.047Cu1Ba2Hg1 - Low (oxygen < 4.101)
4. O4.066Cu1Ba2Hg1 - Low (oxygen < 4.101)
5. O4.081Cu1Ba2Hg1 - Low (oxygen < 4.101)
6. O4.085Cu1Ba2Hg1 - Low (oxygen < 4.101)
7. O4.160Cu1Ba2Hg1 - High (oxygen > 4.101)
8. O4.180Cu1Ba2Hg1 - High (oxygen > 4.101)

Also other unmeasurable CuBaHg materials with oxygen > 4.101: High
With oxygen < 4.101: Low

## Step D: Final pick
Among unmeasurable materials predicted high:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.160Cu1Ba2Hg1 - 4 elements
4. O4.180Cu1Ba2Hg1 - 4 elements

Choose one with fewest elements: O4.160Cu1Ba2Hg1 or O4.180Cu1Ba2Hg1 (both 4 elements)
Break ties by higher expected Tc: Both have similar oxygen content, Tc likely similar (~97 K). Choose O4.180Cu1Ba2Hg1 (slightly higher oxygen).

## Budget Tracking
Starting budget: 45 units
Used: 45 units (32 search + 13 measure)
Remaining: 0 units