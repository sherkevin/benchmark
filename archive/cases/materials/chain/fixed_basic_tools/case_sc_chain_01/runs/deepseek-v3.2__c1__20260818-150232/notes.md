# Superconductor Tc Study Notes

## Step A - Series Peak Analysis

Homologous doping series measurements (O6.XXXCa1Cu2Ba2Hg1):

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

**Maximum Tc (T*)**: 127.0 K
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have Tc = 127.0 K)

**Threshold1**: T* - 32 = 127 - 32 = 95 K

## Step B - Filter Family B (Hg-containing, exactly 4 elements)

Materials measured in O4.XXXCu1Ba2Hg1 series:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (>= 95)
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓ (>= 95)
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓ (>= 95)
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ (>= 95)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓ (>= 95)
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓ (>= 95)
7. O4.047Cu1Ba2Hg1: Tc = 95.0 K ✓ (>= 95)
8. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗ (< 95)

**Q1 (qualifying materials so far)**:
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

**Still need to measure for Step B completeness**:
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1  
- O4.085Cu1Ba2Hg1

## Step C - Induce Rule for Unmeasurable Materials

**Available data for rule induction**:
1. O6 series (5 elements: O, Ca, Cu, Ba, Hg): Tcs from 92-127 K
2. O4 series (4 elements: O, Cu, Ba, Hg): Tcs from 94.2-98.0 K

**Unmeasurable materials to predict**:
1. O6.160Ca1Cu2Ba2Hg1 - similar to O6 series
2. O6.270Ca1Cu2Ba2Hg1 - similar to O6 series  
3. O4.180Cu1Ba2Hg1 - similar to O4 series
4. O8Ca0.850Cu2.200Ba2Tl1.800 - contains Tl, not Hg
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - contains Tl and Pb, not Hg
6. O8Cu3Y1Ba1.900La0.100 - contains Y and La
7. O8Cu4Y1Ba1.870La0.130 - contains Y and La

**Budget status**: 18/18 measurements used. Cannot measure remaining O4 materials or other compositions.

## Step D - Final Pick

Among unmeasurable materials predicted HIGH, pick one with fewest elements (break ties by higher expected Tc).

## Final Analysis and Answer

**Step A Complete**:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K

**Step B (Partial)**:
- Reported 7 qualifying materials with Tc >= 95 K
- Could not measure O4.101, O4.103, O4.085 due to budget constraints
- Only reporting measured materials that qualify

**Step C Rule Induction**:
- All measured materials contain Cu, Ba, and Hg
- All measured materials have Tc >= 90 K (high)
- Induced rule: Materials with Cu, Ba, and Hg simultaneously are high Tc
- This is a multi-factor rule (three elements required)
- Predicts unmeasurable materials with Cu, Ba, Hg as HIGH, others as LOW

**Step D Final Pick**:
- Among unmeasurable materials predicted HIGH: O6.160Ca1Cu2Ba2Hg1 (5 elements), O6.270Ca1Cu2Ba2Hg1 (5 elements), O4.180Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 has fewest elements (4) → selected as final answer

**Answer written to answer.json**