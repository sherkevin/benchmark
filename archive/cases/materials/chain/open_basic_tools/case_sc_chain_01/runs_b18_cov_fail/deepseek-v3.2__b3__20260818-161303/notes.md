# Superconductor Tc Study Notes

## Task Overview
- Starting budget: 18 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low

## Step A: Series Peak Analysis
Goal: Find and measure homologous doping series members (same element set, varying oxygen) to identify maximum Tc (T*).

## Step B: Filter Family B
Goal: Find materials containing Hg with exactly 4 elements, report all with Tc >= threshold1 (where threshold1 = T* - 32).

## Step C: Extrapolate Rule
Goal: Induce rule from measurable materials to predict high/low for unmeasurable ones.

## Step D: Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (tie break by higher expected Tc).

## Budget Tracking
Initial budget: 18 units
Used: 18 units (search_db Hg page 1, search_db Ca page 1, search_db Ca page 2, search_db Ca page 3, search_db Ca page 4, search_db Ca page 5, measure_tc O6.080Ca1Cu2Ba2Hg1, measure_tc O6.100Ca1Cu2Ba2Hg1, measure_tc O6.130Ca1Cu2Ba2Hg1, measure_tc O6.140Ca1Cu2Ba2Hg1, measure_tc O6.160Ca1Cu2Ba2Hg1, measure_tc O6.190Ca1Cu2Ba2Hg1, measure_tc O6.210Ca1Cu2Ba2Hg1, measure_tc O6.220Ca1Cu2Ba2Hg1, measure_tc O6.240Ca1Cu2Ba2Hg1, measure_tc O6.270Ca1Cu2Ba2Hg1, measure_tc O6.300Ca1Cu2Ba2Hg1, measure_tc O6.310Ca1Cu2Ba2Hg1, search_db Hg page 2, measure_tc O4.047Cu1Ba2Hg1)
Remaining: 0 units

## Step A Results
Peak Tc = 127.0 K for composition O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K
threshold1 = T* - 32 = 95.0 K

## Step B Results
Found Hg-containing materials with exactly 4 elements:
1. O4.047Cu1Ba2Hg1: Tc = 95.0 K (measured, qualifies)
2. O4.066Cu1Ba2Hg1: Not measured (unknown)
3. O4.081Cu1Ba2Hg1: Not measured (unknown)
4. O4.085Cu1Ba2Hg1: Not measured (unknown)
5. O4.096Cu1Ba2Hg1: Not measured (unknown)
6. O4.097Cu1Ba2Hg1: Not measured (unknown)

Only O4.047Cu1Ba2Hg1 confirmed to qualify (Tc >= 95.0 K).

## Step C Results
Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (no sample exists)
2. O6.270Ca1Cu2Ba2Hg1 (no sample exists)

Rule induced: Tc follows parabolic trend with oxygen content in Hg-Cu-Ba-Ca-O systems, peaking around O6.2.

## Step D Results
Among unmeasurable materials predicted high (both O6.160 and O6.270), both have 5 elements (tie). O6.160 has oxygen 6.16 closer to peak (6.21-6.22) than O6.270 (6.27), so expected higher Tc. Selected O6.160Ca1Cu2Ba2Hg1.

## Final Answer
Written to answer.json