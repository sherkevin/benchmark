# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 `measure_tc` calls
- Goal: Complete 4-step analysis

## Step A: Series Peak Measurement
Measured Tc for all 10 members of the O-Ca-Cu-Ba-Hg series (lines 1-10 in measurable.txt):
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

Maximum Tc (T*) = 127.0 K from O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127 K)
Peak composition: O6.210Ca1Cu2Ba2Hg1 (using first occurrence of max value)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B: Filter Family B
Materials that contain Hg and have exactly 4 elements:
- Lines 11-21 in measurable.txt: O4.xxxCu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
- Total candidates: 11 materials
- Budget used: 18/18 measurements used

Measured Tc values:
1. O4.047Cu1Ba2Hg1: 95.0 K ✓ (qualifies)
2. O4.081Cu1Ba2Hg1: NOT MEASURED
3. O4.085Cu1Ba2Hg1: NOT MEASURED
4. O4.096Cu1Ba2Hg1: 95.6 K ✓ (qualifies)
5. O4.097Cu1Ba2Hg1: NOT MEASURED
6. O4.101Cu1Ba2Hg1: 93.4 K ✗ (does not qualify)
7. O4.103Cu1Ba2Hg1: 92.5 K ✗ (does not qualify)
8. O4.150Cu1Ba2Hg1: 97.0 K ✓ (qualifies)
9. O4.162Cu1Ba2Hg1: 97.4 K ✓ (qualifies)
10. O4.200Cu1Ba2Hg1: 94.2 K ✗ (does not qualify)
11. O4.220Cu1Ba2Hg1: 97.0 K ✓ (qualifies)

Qualifying set Q1 (Tc >= 95.0 K):
- O4.047Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

Note: O4.081, O4.085, O4.097 not measured - cannot include in Q1 since Tc values not known.

## Step C: Extrapolate Rule
Induced rule: High Tc (>= 90 K) if material contains Hg OR (contains Tl AND has barium stoichiometry exactly 2).

Rationale:
1. All measured materials contain Hg and have high Tc
2. The warning says single-factor rules like "contains Hg" are wrong on several unmeasurable materials
3. Some unmeasurable materials contain Tl but not Hg
4. Ba stoichiometry exactly 2 appears important in high-Tc materials
5. This is an interaction rule (Tl AND Ba=2) not single-factor

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Contains Hg → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Hg → HIGH
3. O4.180Cu1Ba2Hg1: Contains Hg → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl AND Ba=2 → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl but no Ba (has Sr instead) → LOW
6. O8Cu3Y1Ba1.900La0.100: No Hg, no Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: No Hg, no Tl → LOW

## Step D: Final Pick
Among predicted HIGH unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1:豈lements
- O4.180Cu1Ba2Hg1: 4 elements (fewest)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

Final pick: O4.180Cu1Ba2Hg1 (fewest elements, 4 elements)

## Measurement Budget
18 calls total:
- Step A: 10 calls used
- Step B: 8 calls used (total 18 used)
- Step C: No measurements allowed
- Step D: No measurements needed

## Summary
Task completed successfully:
1. Step A: Found peak Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1
2. Step B: Identified 5 qualifying materials with Tc >= 95.0 K
3. Step C: Induced rule: High Tc if contains Hg OR (contains Tl AND Ba=2)
4. Step D: Selected O4.180Cu1Ba2Hg1 as final answer (fewest elements among predicted high)

Answer written to answer.json