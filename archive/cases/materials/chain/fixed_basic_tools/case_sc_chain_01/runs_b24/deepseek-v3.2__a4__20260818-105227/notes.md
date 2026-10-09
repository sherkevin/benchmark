# Superconductor Tc Study Notes

## Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 24 measure_tc calls
- High Tc definition: >= 90 K

## Step A - Series Peak
Need to measure homologous doping series (same element set, varying oxygen). Looking at measurable.txt:
- First 10 materials appear to be the series: O6.080Ca1Cu2Ba2Hg1 through O6.310Ca1Cu2Ba2Hg1
- These all have elements: O, Ca, Cu, Ba, Hg (5 elements)
- Oxygen content varies from 6.080 to 6.310

Need to measure all 10 to find maximum Tc (T*).

## Step B - Filter Family B
Threshold1 = T* - 32
Need materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= threshold1

From measurable.txt, materials with Hg and exactly 4 elements appear to be:
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1

These all have elements: O, Cu, Ba, Hg (4 elements)

## Step C - Extrapolate
Need to induce a rule to separate high/low Tc using measurable materials, then predict for unmeasurable materials.

Unmeasurable materials (7):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

## Step D - Final Pick
Among unmeasurable materials predicted high in step C, pick one with fewest elements (break ties by higher expected Tc).

## Step A Results
Measured all 10 materials in homologous series:
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

Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (choosing first of tied pair)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Results
Measured all 11 materials with Hg and exactly 4 elements:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (qualifies)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ (qualifies)
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ (qualifies)
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ (qualifies)
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ (qualifies)
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ (qualifies)
7. O4.047Cu1Ba2Hg1: 95.0 K ✓ (qualifies)
8. O4.200Cu1Ba2Hg1: 94.2 K ✗ (does not qualify)
9. O4.101Cu1Ba2Hg1: 93.4 K ✗ (does not qualify)
10. O4.103Cu1Ba2Hg1: 92.5 K ✗ (does not qualify)
11. O4.085Cu1Ba2Hg1: 92.0 K ✗ (does not qualify)

Q1 = First 7 materials (all with Tc >= 95.0 K)

## Additional Measurements for Rule Induction
Measured 3 more materials:
1. O7Cu3Ba2La1: 89.6 K (LOW) - Elements: O, Cu, Ba, La (4 elements, no Hg)
2. O6Cu1Ba2Hg1Tl1: 0.0 K (LOW) - Elements: O, Cu, Ba, Hg, Tl (5 elements, contains Hg and Tl)
3. O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH) - Elements: O, Ca, Cu, Ba, Tl (5 elements, contains Tl but not Hg)

## Induced Rule for Step C
Based on measurement data, the rule for high Tc (>= 90 K) is:
**A material has high Tc if it contains barium (Ba) and contains exactly one of mercury (Hg) or thallium (Tl).**

### Rule Validation Against Measured Data:
- All high Tc materials: Have Ba and exactly one of Hg/Tl
- O7Cu3Ba2La1 (low): Has Ba but neither Hg nor Tl (0, not exactly 1)
- O6Cu1Ba2Hg1Tl1 (low): Has Ba but both Hg and Tl (2, not exactly 1)

## Predictions for Unmeasurable Materials (Step C):
1. O6.160Ca1Cu2Ba2Hg1: Contains Ba ✓, Hg ✓, Tl ✗ → exactly one → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Ba ✓, Hg ✓, Tl ✗ → exactly one → HIGH
3. O4.180Cu1Ba2Hg1: Contains Ba ✓, Hg ✓, Tl ✗ → exactly one → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ba ✓, Hg ✗, Tl ✓ → exactly one → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ba ✗ (has Sr), Hg ✗, Tl ✓ → no Ba → LOW
6. O8Cu3Y1Ba1.900La0.100: Contains Ba ✓, Hg ✗, Tl ✗ → zero (not exactly one) → LOW
7. O8Cu4Y1Ba1.870La0.130: Contains Ba ✓, Hg ✗, Tl ✗ → zero (not exactly one) → LOW

## Step D - Final Pick
Among materials predicted HIGH (#1-4):
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements (fewest)
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final pick: O4.180Cu1Ba2Hg1** (4 elements, no tie to break)

## Budget Status
- Total measurements used: 24/24
- Budget exhausted