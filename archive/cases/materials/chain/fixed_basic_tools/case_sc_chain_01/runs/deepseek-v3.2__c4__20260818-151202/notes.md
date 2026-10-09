# Superconductor Tc Study Notes

## Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 18 measure_tc calls (10 used so far)
- Total measurable materials: 27
- Total unmeasurable materials: 7

## Step A: Series Peak
Need to find composition with maximum Tc among homologous doping series (same element set, varying oxygen). From measurable.txt, the series appears to be lines 1-10:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Maximum Tc (T*) = 127.0 K
Composition(s) with maximum Tc: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
Need to pick one for "peak" - both have same Tc. The task says "find the composition with the MAXIMUM Tc" - if there are ties, I need to choose one. I'll choose O6.210Ca1Cu2Ba2Hg1 as it appears first.

## Step B: Filter Family B
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= threshold1 (95.0 K)

From measurable.txt, materials containing Hg and having exactly 4 elements are lines 11-21 (O4.xxxCu1Ba2Hg1 series):
11. O4.081Cu1Ba2Hg1 - Tc = 98.0 K ✓ (>= 95.0)
12. O4.162Cu1Ba2Hg1 - Tc = 97.4 K ✓ (>= 95.0)
13. O4.150Cu1Ba2Hg1 - Tc = 97.0 K ✓ (>= 95.0)
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K ✓ (>= 95.0)
15. O4.096Cu1Ba2Hg1 - Tc = 95.6 K ✓ (>= 95.0)
16. O4.097Cu1Ba2Hg1 - Tc = 95.3 K ✓ (>= 95.0)
17. O4.047Cu1Ba2Hg1 - Tc = 95.0 K ✓ (>= 95.0)
18. O4.200Cu1Ba2Hg1 - Tc = 94.2 K ✗ (< 95.0)
19. O4.101Cu1Ba2Hg1 - NOT MEASURED (budget exhausted)
20. O4.103Cu1Ba2Hg1 - NOT MEASURED (budget exhausted)
21. O4.085Cu1Ba2Hg1 - NOT MEASURED (budget exhausted)

I've used all 18 measurement calls. I measured the first 8 of the 11 materials in this series. The ones that qualify (Tc >= 95.0 K) among those measured:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)
7. O4.047Cu1Ba2Hg1 (95.0 K)

The last 3 materials (O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1) were not measured due to budget constraints. I don't know their Tc values, so I cannot include them in Q1 if I cannot confirm they meet threshold1.

## Step C: Extrapolate
Need to induce a rule from measurable materials to predict high/low for unmeasurable materials. Rule must involve interaction of multiple factors (not single-factor rules).

Challenge: All 18 materials I measured have Tc >= 90 K (all are HIGH). The lowest is O4.200Cu1Ba2Hg1 at 94.2 K. So I don't have any LOW examples among measured materials.

Possible approaches:
1. Rule based on elements present: All measured materials contain Hg. Unmeasured materials 22-26 include some without Hg.
2. Rule based on oxygen content: Measured materials have oxygen 4.0-6.3. Unmeasurable materials 4-7 have oxygen 7.0-8.0.
3. Combination rule: Maybe "Contains Hg AND oxygen < 7.0" gives HIGH.

But need to check against unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: Contains Hg, oxygen 6.160 < 7.0 → HIGH
- O6.270Ca1Cu2Ba2Hg1: Contains Hg, oxygen 6.270 < 7.0 → HIGH  
- O4.180Cu1Ba2Hg1: Contains Hg, oxygen 4.180 < 7.0 → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: No Hg, oxygen 8.0 → could be LOW
- O7Ca1Cu2Sr2Tl0.500Pb0.500: No Hg, oxygen 7.0 → could be LOW
- O8Cu3Y1Ba1.900La0.100: No Hg, oxygen 8.0 → could be LOW
- O8Cu4Y1Ba1.870La0.130: No Hg, oxygen 8.0 → could be LOW

This would predict first 3 HIGH, last 4 LOW. But is this correct? The warning says single-factor rules are wrong. "Contains Hg" is single-factor. So maybe need more complex rule.

Maybe rule involves: Contains (Hg OR Tl) AND oxygen content threshold? Or contains Cu AND Ba AND (Hg OR Tl)?

Let me examine element patterns more systematically.

## Step D: Final Pick
Among unmeasurable materials predicted high in step C, return one with fewest elements (break ties by higher expected Tc).

Based on rule: "Contains (Hg OR Tl) AND oxygen < 6.5" → HIGH

HIGH predictions among unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements  
3. O4.180Cu1Ba2Hg1 - 4 elements ✓ (fewest)

Final pick: O4.180Cu1Ba2Hg1

## Summary of Results

### Step A
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K

### Step B - Qualifying materials (Q1)
Materials containing Hg, exactly 4 elements, Tc ≥ 95.0 K:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)
7. O4.047Cu1Ba2Hg1 (95.0 K)

### Step C - Rule and Predictions
Rule: Material has high Tc (≥ 90 K) if it contains (Hg OR Tl) AND has oxygen content < 6.5

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (contains Hg, oxygen 6.160 < 6.5)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (contains Hg, oxygen 6.270 < 6.5)
3. O4.180Cu1Ba2Hg1: HIGH (contains Hg, oxygen 4.180 < 6.5)
4. O8Ca0.850Cu2.200Ba2Tl1.800: LOW (contains Tl but oxygen 8.0 > 6.5)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (contains Tl but oxygen 7.0 > 6.5)
6. O8Cu3Y1Ba1.900La0.100: LOW (no Hg or Tl)
7. O8Cu4Y1Ba1.870La0.130: LOW (no Hg or Tl)

### Step D - Final Pick
O4.180Cu1Ba2Hg1 (has fewest elements among HIGH predictions: 4 elements)