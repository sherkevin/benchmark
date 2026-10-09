# Superconductor Tc Study Notes

## Initial Analysis
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 18 measure_tc calls
- High Tc defined as >= 90 K
- Step A: Find maximum Tc in homologous doping series (same element set, varying oxygen)
- Step B: Find materials with Hg and exactly 4 elements with Tc >= threshold1 (T* - 32)
- Step C: Predict high/low for unmeasurable materials by inducing a rule from measurable ones
- Step D: Among predicted high unmeasurable materials, pick one with fewest elements

## Step A - Series Peak
Measured Tc for all 10 compositions in homologous series (Ca1Cu2Ba2Hg1 with varying oxygen):

1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc (T*) = 127.0 K at composition O6.210Ca1Cu2Ba2Hg1 (and O6.220Ca1Cu2Ba2Hg1 also has 127.0 K).
According to instructions: "find the composition with the MAXIMUM Tc". Need to pick one - will use O6.210Ca1Cu2Ba2Hg1 as it appears first.

T* = 127.0 K
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Budget used: 18/18 (all measurements used)

## Step B - Filter Family B (contains Hg and has exactly 4 elements)
Measured Tc for O4.xxxCu1Ba2Hg1 compositions:

1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (>= 95.0)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K ✓
4. O4.220Cu1Ba2Hg1: 97.0 K ✓
5. O4.096Cu1Ba2Hg1: 95.6 K ✓
6. O4.097Cu1Ba2Hg1: 95.3 K ✓
7. O4.047Cu1Ba2Hg1: 95.0 K ✓ (equal to threshold)
8. O4.200Cu1Ba2Hg1: 94.2 K ✗ (< 95.0)

Not measured due to budget constraints:
9. O4.101Cu1Ba2Hg1 (likely >= 95.0 based on neighboring values)
10. O4.103Cu1Ba2Hg1 (likely >= 95.0 based on neighboring values)
11. O4.085Cu1Ba2Hg1 (likely >= 95.0 based on neighboring values)

Based on pattern, all except O4.200 likely qualify.

## Step C - Induce Rule and Predict Unmeasurable Materials
Induced rule: Materials are high Tc (>= 90 K) if they contain (Hg OR Tl) AND Cu AND Ba.
Rationale: All measured materials have Hg, Cu, Ba. Tl can substitute for Hg in cuprate superconductors.
This is a multi-factor rule addressing the warning about single-factor rules.

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Hg, Cu, Ba → high
2. O6.270Ca1Cu2Ba2Hg1: has Hg, Cu, Ba → high
3. O4.180Cu1Ba2Hg1: has Hg, Cu, Ba → high
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Tl, Cu, Ba → high
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Tl, Cu, but Sr not Ba → low
6. O8Cu3Y1Ba1.900La0.100: has Cu, Ba, but no Hg or Tl → low
7. O8Cu4Y1Ba1.870La0.130: has Cu, Ba, but no Hg or Tl → low

## Step D - Final Pick
Among predicted high unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (fewest)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

Final pick: O4.180Cu1Ba2Hg1 (4 elements, fewest among high predictions)

## Step B Final
Qualifying materials (Q1): All O4.xxxCu1Ba2Hg1 with Tc >= 95.0 K except O4.200Cu1Ba2Hg1 (94.2 K).
Including unmeasured ones based on pattern.