# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, low Tc < 90 K
- Measurement budget: 18 `measure_tc` calls
- Must use `measure_tc` for every Tc value cited

## Step A: Find maximum Tc in homologous doping series
The seed belongs to a homologous doping series (same element set, varying oxygen). Need to measure all materials in `data/measurable.txt` that have the same element set as seed: O, Ca, Cu, Ba, Hg. Looking at the file:
- Lines 1-10 appear to be the homologous series with varying oxygen content
- Other lines have different element compositions

First, let me identify which materials belong to the homologous series with seed O6.080Ca1Cu2Ba2Hg1. The seed has elements: O, Ca, Cu, Ba, Hg.

From measurable.txt:
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1 
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

These 10 materials all have the exact same element set: O, Ca, Cu, Ba, Hg. This is the homologous series.

Need to measure Tc for all 10 to find maximum Tc (T*). Budget: 18 total measurements available.

## Step B: Filter materials with Hg and exactly 4 elements
Need to find all materials in measurable.txt that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= threshold1 (where threshold1 = T* - 32)

## Step C: Predict high/low for unmeasurable materials
Need to induce a rule based on measurable materials to predict high/low for unmeasurable materials. Rule must involve interaction of multiple factors.

## Step D: Final pick
Among unmeasurable materials predicted high, pick one with fewest elements (break ties by higher expected Tc).

## Step A Results (Homologous Series)
All 10 materials in homologous series measured:

1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

**Maximum Tc (T*) = 127.0 K** (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Since both have same Tc, need to pick one as peak composition. Let's use O6.210Ca1Cu2Ba2Hg1 as it appears first.

**Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

## Plan
1. ~~Measure all 10 materials in homologous series (Step A)~~
2. ~~Calculate T* and threshold1~~
3. Identify materials with Hg and exactly 4 elements
4. Measure their Tc to find Q1 (Step B)
5. Analyze patterns to induce rule for high/low prediction (Step C)
6. Apply rule to unmeasurable materials
7. Select final material (Step D)

Remaining budget: 8 measurements (used 10 so far, total budget 18)