# Notes for Superconductor Tc Study

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 18 measure_tc calls
- Need to complete 4 steps (A-D)

## Step A - Series Peak
Need to measure series members in measurable.txt and find maximum Tc (T*).

From measurable.txt, I see two main series:
1. O6.xxxCa1Cu2Ba2Hg1 (lines 1-10) - homologous doping series with varying oxygen
2. O4.xxxCu1Ba2Hg1 (lines 11-21) - another series with varying oxygen
3. Other individual compositions (lines 22-27)

The seed O6.080Ca1Cu2Ba2Hg1 suggests the first series is the homologous doping series mentioned in Step A.

I should measure all compositions in measurable.txt to find the maximum Tc, but need to be careful with budget (18 calls total for all steps).

## Plan
1. First measure the homologous series (O6.xxxCa1Cu2Ba2Hg1) to find T*
2. Then measure other compositions to find Q1 set for Step B
3. Analyze patterns for Step C rule induction
4. Make predictions for unmeasurable materials
5. Select final pick

## All Measurements (18/18 budget used)

### Homologous Series (O6.xxxCa1Cu2Ba2Hg1):
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

### O4.xxxCu1Ba2Hg1 series (partial measurements):
11. O4.081Cu1Ba2Hg1: Tc = 98.0 K
12. O4.162Cu1Ba2Hg1: Tc = 97.4 K
13. O4.150Cu1Ba2Hg1: Tc = 97.0 K
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K

### Other compositions:
15. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
16. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K
17. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
18. O7Cu3Ba2La1: Tc = 89.6 K

## Step A Analysis
Maximum Tc (T*) found: 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

I didn't measure all compositions, but 127.0 K seems to be the highest value observed. The O10Ca2Cu3Ba2Tl2 has 118.97 K which is lower. The O4.xxx series are around 97-98 K. So T* = 127.0 K.

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Analysis
Need to find materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= threshold1 (95.0 K)

From measurable.txt:
- Lines   1-10: O6.xxxCa1Cu2Ba2Hg1 - 5 elements (contains Hg but not exactly 4 elements)
- Lines 11-21: O4.xxxCu1Ba2Hg1 - 4 elements (contains Hg, exactly 4 elements)
- Lines 22-27: Other compositions - either don't contain Hg or don't have exactly 4 elements

Of the O4.xxxCu1Ba2Hg1 series, I measured 4 compositions:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K (>= 95.0) ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K (>= 95.0) ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0) ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0) ✓

I didn't measure the other 7 compositions in this series due to budget constraints, so I can only include these 4 in Q1.

Potential Q1 members (based on measurements):
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

## Step C Analysis
Need to induce a rule that separates high (Tc >= 90 K) from low Tc using measurable materials, then predict for unmeasurable materials.

Warning: single-factor rules (e.g. "contains Hg" or "has 5 elements") are wrong on several of them — the governing rule is an interaction of multiple factors.

### Analysis of Measurements:
High Tc (>= 90 K):
1. O6.080Ca1Cu2Ba2Hg1 (92.0): Hg, no Tl
2. O6.100Ca1Cu2Ba2Hg1 (92.5): Hg, no Tl
3. O6.130Ca1Cu2Ba2Hg1 (102.0): Hg, no Tl
4. O6.140Ca1Cu2Ba2Hg1 (105.0): Hg, no Tl
5. O6.190Ca1Cu2Ba2Hg1 (122.0): Hg, no Tl
6. O6.210Ca1Cu2Ba2Hg1 (127.0): Hg, no Tl
7. O6.220Ca1Cu2Ba2Hg1 (127.0): Hg, no Tl
8. O6.240Ca1Cu2Ba2Hg1 (123.0): Hg, no Tl
9. O6.300Ca1Cu2Ba2Hg1 (100.0): Hg, no Tl
10. O6.310Ca1Cu2Ba2Hg1 (98.0): Hg, no Tl
11. O4.081Cu1Ba2Hg1 (98.0): Hg, no Tl
12. O4.162Cu1Ba2Hg1 (97.4): Hg, no Tl
13. O4.150Cu1Ba2Hg1 (97.0): Hg, no Tl
14. O4.220Cu1Ba2Hg1 (97.0): Hg, no Tl
15. O10Ca2Cu3Ba2Tl2 (118.97): Tl with Ca and Ba, no Hg

Low Tc (< 90 K):
1. O6Cu1Ba2Hg1Tl1 (0.0): Hg AND Tl
2. O6Cu1Cd0.250Ba2Tl1.750 (88.0): Tl but no Ca or Hg
3. O7Cu3Ba2La1 (89.6): no Hg, no Tl, no Ca/Ba/Tl combo

### Induced Rule:
Based on patterns, I propose:
**High Tc if: (contains Hg AND NOT Tl) OR (contains Ca AND Ba AND Tl)**

This rule correctly classifies all measured materials:
- Hg-containing without Tl: high ✓
- Ca-Ba-Tl combination: high ✓ (O10Ca2Cu3Ba2Tl2)
- Hg AND Tl together: low ✓ (O6Cu1Ba2Hg1Tl1)
- Tl without Ca and Ba: low ✓ (O6Cu1Cd0.250Ba2Tl1.750)
- No Hg, no Ca/Ba/Tl: low ✓ (O7Cu3Ba2La1)

### Predictions for Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg without Tl → HIGH
3. O4.180Cu1Ba2Hg1: Hg without Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ca, Ba, Tl → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca, Tl but NO Ba (has Sr) → LOW
6. O8Cu3Y1Ba1.900La0.100: No Hg, no Ca/Ba/Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: No Hg, no Ca/Ba/Tl → LOW

## Step D Analysis
Among unmeasurable materials predicted HIGH:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

O4.180Cu1Ba2Hg1 has fewest elements (4 vs 5). No tie to break.

## Final Answer Compiled
- peak: O6.210Ca1Cu2Ba2Hg1 (max Tc = 127.0 K)
- peak_tc: 127.0
- threshold: 95.0
- qualifying: ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]
- predictions: as above
- final: O4.180Cu1Ba2Hg1
- rule: "High Tc if: (contains Hg AND NOT Tl) OR (contains Ca AND Ba AND Tl)"

Answer written to answer.json.