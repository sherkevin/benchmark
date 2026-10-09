# Superconductor Tc Study Notes

## Initial Setup
- Starting budget:ブロック引用 26 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc definition: Tc >= 90 K
- Tools: measure_tc (1 unit each), search_db (1 unit per page)

## Step A - Series Peak
Goal: Find homologous doping series (same element set, varying oxygen), find composition with maximum Tc (T*).

Results from searching "Ca" (5 pages, 5 budget units):
Found homologous series: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measurements (10 budget units):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓ (MAX)
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓ (tie)
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓

Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (chosen from tie)
Threshold1 = T* - 32 = 95.0 K

## Step B - Filter Family B
Goal: Find materials containing Hg with exactly 4 elements, Tc >= threshold1 (95.0 K).

Search for "Hg" (9 pages, 9 budget units): Found 27 total matches.
4-element Hg materials (O4.XXXCu1Ba2Hg1 series, 14 total):
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

Measurements (2 budget units, last available):
1. O4.047Cu1Ba2Hg1: Tc = 95.0 K ✓ (qualifies, exactly at threshold)
2. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (qualifies)

Other 5-element Hg materials (our homologous series, O6.XXXCa1Cu2Ba2Hg1) do NOT have exactly 4 elements.

Problem: Cannot measure all 14 materials due to budget exhaustion (26/26 used).
Need to induce rule from available data to predict which ones qualify.

## Step C - Extrapolate
Goal: Induce rule from measurable materials, predict high/low for unmeasurable ones.

Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 (from series)
2. O6.270Ca1Cu2Ba2Hg1 (from series)
3. All other O4.XXXCu1Ba2Hg1 materials (12 unmeasured) - unknown if they have samples

## Step D - Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements.

Budget status: Exhausted (26/26 units used)

## Final Analysis and Decisions

### Step A - Series Peak
- Maximum Tc (T*) = 127.0 K from O6.210Ca1Cu2Ba2Hg1 (tied with O6.220Ca1Cu2Ba2Hg1)
- Chose O6.210Ca1Cu2Ba2Hg1 as peak composition
- Threshold1 = 127.0 - 32 = 95.0 K

### Step B - Filter Family B
- Found 14 materials with exactly 4 elements containing Hg: O4.XXXCu1Ba2Hg1 series
- Measured 2 due to budget constraints: O4.047Cu1Ba2Hg1 (95.0 K) and O4.081Cu1Ba2Hg1 (98.0 K)
- Both meet threshold1 (>= 95.0 K)
- Attempted to measure O4.066Cu1Ba2Hg1 but got error "unknown composition"
- With exhausted budget, can only confirm 2 qualifying materials
- Final qualifying list includes only confirmed measurements: O4.047Cu1Ba2Hg1 and O4.081Cu1Ba2Hg1

### Step C - Extrapolate
- Confirmed unmeasurable materials: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 (from measurement attempts returning "no sample exists")
- Based on homologous series trend (parabolic with oxygen), both should be high (Tc >= 90 K)
- O6.160: between O6.140 (105.0 K) and O6.190 (122.0 K) - predicted high
- O6.270: between O6.240 (123.0 K) and O6.300 (100.0 K) - predicted high
- Rule induced: Hg-containing superconductors exhibit high Tc when oxygen content is optimized for their series

### Step D - Final Pick
- Unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- Both have 5 elements (tie)
- O6.160 likely has higher expected Tc (closer to peak at O6.210-O6.220) than O6.270 (after peak)
- Chose O6.160Ca1Cu2Ba2Hg1 as final answer

## Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1, Tc = 127.0 K
- Threshold: 95.0 K
- Qualifying: O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- Predictions: O6.160Ca1Cu2Ba2Hg1 (high), O6.270Ca1Cu2Ba2Hg1 (high)
- Final: O6.160Ca1Cu2Ba2Hg1
- Rule: Hg-containing superconductors exhibit high Tc (>= 90 K) when oxygen content is properly optimized