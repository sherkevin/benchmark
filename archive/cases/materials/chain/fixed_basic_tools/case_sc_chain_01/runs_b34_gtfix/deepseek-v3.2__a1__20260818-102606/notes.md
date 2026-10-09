# Superconductor Tc Study Notes - COMPLETE

## Task Summary
Successfully completed all steps within measurement budget (27/34 calls used).

## Step A - Series Peak ✓
- Measured all 26 compositions in measurable.txt
- Maximum Tc (T*) = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B - Filter Family B ✓
- Threshold1 = T* - 32 = 95.0 K
- Q1 set: 7 compositions meeting criteria (contain Hg, exactly 4 elements, Tc >= 95.0 K)
  - O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1

## Step C - Extrapolate Rule ✓
- Induced rule from measurable compositions
- Rule: High Tc if composition contains Cu AND Ba AND either Hg or Tl (but not both) AND does not contain La, Y, or Cd
- Applied rule to predict high/low for all 7 unmeasurable compositions

## Step D - Final Pick ✓
- Among unmeasurable compositions predicted HIGH, selected O4.180Cu1Ba2Hg1
- Reason: Fewest elements (4 elements) among HIGH predictions

## Budget Usage
- Measurements made: unmeasurable compositions (26 successful, 1 failed attempt)
- Total calls: 27 out of 34 budget
- Remaining budget: 7 calls unused

## Measurement Results (Step A)
All 27 compositions measured. Tc values:

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
11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O4.150Cu1Ba2Hg1: 97.0 K
14. O4.220Cu1Ba2Hg1: 97.0 K
15. O4.096Cu1Ba2Hg1: 95.6 K
16. O4.097Cu1Ba2Hg1: 95.3 K
17. O4.047Cu1Ba2Hg1: 95.0 K
18. O4.200Cu1Ba2Hg1: 94.2 K
19. O4.101Cu1Ba2Hg1: 93.4 K
20. O4.103Cu1Ba2Hg1: 92.5 K
21. O4.085Cu1Ba2Hg1: 92.0 K
22. O6Cu1Ba2Hg1Tl1: 0.0 K
23. O10Ca2Cu3Ba2Tl2: 118.97 K
24. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
25. O7Cu3Ba2La1: 89.6 K
26. O7Cu3Y0.200Ba2La0.800: 81.0 K

**Maximum Tc (T*)**: 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

## Step B Analysis
**Threshold1** = T* - 32 = 127.0 - 32 = 95.0 K

**Compositions meeting Step B criteria** (contain Hg, exactly 4 elements, Tc >= 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.097Cu1Ba2Hg1 (95.3 K)
7. O4.047Cu1Ba2Hg1 (95.0 K)

**Q1 set**: These 7 compositions

## Step C Analysis - Rule Induction

**Observations from measurable compositions:**
- 21 high Tc (>= 90 K), 5 low Tc (< 90 K)
- All high Tc compositions contain Cu and Ba
- All high Tc compositions contain either Hg OR Tl (but not both together)
- No high Tc composition contains La, Y, or Cd
- Low Tc compositions either:
  - Contain both Hg AND Tl together (O6Cu1Ba2Hg1Tl1)
  - Contain Cd AND Tl together (O6Cu1Cd0.250Ba2Tl1.750)
  - Contain La (O7Cu3Ba2La1)
  - Contain Y AND La (O7Cu3Y0.200Ba2La0.800)

**Rule hypothesis:**
High Tc if:
1. Contains Cu AND Ba
2. Contains either Hg XOR Tl (exclusive or - not both)
3. Does NOT contain La, Y, or Cd
4. For Tl-containing compositions, may need appropriate oxygen stoichiometry

**Applying to unmeasurable compositions:**
1. O6.160Ca1Cu2Ba2Hg1: Cu✓ Ba✓ Hg✓ (no Tl) La✗ Y✗ Cd✗ → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Cu✓ Ba✓ Hg✓ (no Tl) La✗ Y✗ Cd✗ → HIGH  
3. O4.180Cu1Ba2Hg1: Cu✓ Ba✓ Hg✓ (no Tl) La✗ Y✗ Cd✗ → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Cu✓ Ba✓ Tl✓ (no Hg) La✗ Y✗ Cd✗ → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Cu✓ Ba✗ (has Sr) Tl✓ Pb✓ (Pb similar to Cd?) → likely LOW
6. O8Cu3Y1Ba1.900La0.100: Cu✓ Ba✓ Y✓ La✓ → LOW
7. O8Cu4Y1Ba1.870La0.130: Cu✓ Ba✓ Y✓ La✓ → LOW

**Refined rule:**
High Tc if: Contains Cu AND Ba AND (Hg XOR Tl) AND NOT (La OR Y OR Cd OR (Hg AND Tl))

## Step D Analysis - Final Pick

**Unmeasurable compositions predicted HIGH:**
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.180Cu1Ba2Hg1 - 4 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements

**Final pick**: O4.180Cu1Ba2Hg1 (fewest elements - 4)

No tie-breaking needed as it's the only composition with 4 elements among HIGH predictions.

## Summary
- **Peak composition**: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- **Threshold1**: 95.0 K
- **Q1 set**: 7 O4.*Cu1Ba2Hg1 compositions with Tc >= 95.0 K
- **Rule**: High Tc if contains Cu AND Ba AND either Hg or Tl (but not both) AND does not contain La, Y, or Cd
- **Final pick**: O4.180Cu1Ba2Hg1