# Superconductor Tc Study Notes

## Initial Setup
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as >= 90K
- Need to complete four steps: A, B, C, D

## Step A - Series Peak
Need to find homologous doping series (same element set, varying oxygen) to find maximum Tc (T*).

Plan:
1. First measure the seed material Tc to establish baseline
2. Search for materials with same element set (Ca, Cu, Ba, Hg) but varying oxygen
3. Measure all found series members to find maximum Tc

## Step B - Filter Family B
After finding T*, calculate threshold1 = T* - 32
Find materials that:
- Contain Hg
- Have exactly 4 elements
- Tc >= threshold1

## Step C - Extrapolate
Some materials cannot be measured ("no sample exists")
Need to induce a rule from measurable materials to predict high/low for unmeasurable ones
Warning: single-factor rules are wrong on several

## Step D - Final Pick
Among unmeasurable materials predicted HIGH, choose one with:
- Fewest elements
- If tie: higher expected Tc

## Budget Considerations
Total budget 45 units, need to allocate wisely between searches and measurements.

Let's start!

## Step A Progress
1. Measured seed O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (HIGH)
2. Budget used: 1, remaining: 44

Found homologous series members (Ca1Cu2Ba2Hg1 with varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1 (92.0K) ✓
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

Budget after searches: used 9, remaining 36

## Step A Measurements:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K ✓
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K ✓
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K ✓
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample available (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K ✓
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K ✓
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K ✓
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample available (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K ✓
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K ✓

Maximum Tc (T*) = 127.0K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Budget after measurements: used 18, remaining 27

## Step A Complete:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- T* = 127.0K
- Threshold1 = T* - 32 = 95.0K

## Step B - Filter Family B:
Need to find materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= 95.0K

Found 14 materials with exactly 4 elements (all O4.xxxCu1Ba2Hg1):
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

Need to measure their Tc values.
Budget after Hg searches: used 22, remaining 23

## Step B Measurements (4-element Hg materials):
1. O4.047Cu1Ba2Hg1: Unmeasurable
2. O4.066Cu1Ba2Hg1: Unmeasurable
3. O4.081Cu1Ba2Hg1: Tc = 98.0K ✓ (QUALIFIES)
4. O4.085Cu1Ba2Hg1: Tc = 92.0K ✗ (< 95.0K)
5. O4.096Cu1Ba2Hg1: Tc = 95.6K ✓ (QUALIFIES)
6. O4.097Cu1Ba2Hg1: Tc = 95.3K ✓ (QUALIFIES)
7. O4.101Cu1Ba2Hg1: Tc = 93.4K ✗ (< 95.0K)
8. O4.103Cu1Ba2Hg1: Tc = 92.5K ✗ (< 95.0K)
9. O4.150Cu1Ba2Hg1: Tc = 97.0K ✓ (QUALIFIES)
10. O4.160Cu1Ba2Hg1: Unmeasurable
11. O4.162Cu1Ba2Hg1: Tc = 97.4K ✓ (QUALIFIES)
12. O4.180Cu1Ba2Hg1: Unmeasurable
13. O4.200Cu1Ba2Hg1: Tc = 94.2K ✗ (< 95.0K)
14. O4.220Cu1Ba2Hg1: Tc = 97.0K ✓ (QUALIFIES)

Qualifying materials (Q1):
1. O4.081Cu1Ba2Hg1
2. O4.096Cu1Ba2Hg1
3. O4.097Cu1Ba2Hg1
4. O4.150Cu1Ba2Hg1
5. O4.162Cu1Ba2Hg1
6. O4.220Cu1Ba2Hg1

Budget after measurements: used 32, remaining 13

## Step C - Extrapolate:
Need to find unmeasurable materials and predict high/low.

New materials found from searches:
1. O6Cu1Cd0.250Ba2Tl1.750 (from Tl search)
2. O10Ca2Cu3Ba2Tl2 (from various searches)
3. O6Cu1Ba2Hg1Tl1 (from Hg search)
4. O7Ca1Cu2Sr2Tl0.500Pb0.500 (from Ca/Tl/Pb/Sr searches)
5. O8Ca0.850Cu2.200Ba2Tl1.800 (from Tl search)

Unmeasurable materials found so far:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.160Cu1Ba2Hg1
6. O4.180Cu1Ba2Hg1

Need to check if new materials are measurable or not.
Budget after searches: used 40, remaining 5

## New Material Measurements:
1. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0K (LOW)
2. O10Ca2Cu3Ba2Tl2: Tc = 118.97K (HIGH)
3. O6Cu1Ba2Hg1Tl1: Tc = 0.0K (LOW)
4. O7Ca1Cu2Sr2Tl0.500Pb0.500: Unmeasurable
5. O8Ca0.850Cu2.200Ba2Tl1.800: Unmeasurable

## Complete List of Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.160Cu1Ba2Hg1
6. O4.180Cu1Ba2Hg1
7. O7Ca1Cu2Sr2Tl0.500Pb0.500
8. O8Ca0.850Cu2.200Ba2Tl1.800

Total:165.0K (LOW) - 5 elements (O, Cu, Ba, Hg, Tl)
24. O6Cu1Ba2Hg1Tl1: Tc = 0.0K (LOW) - 5 elements (O, Cu, Ba, Hg, Tl)

## Induced Rule:
Materials are HIGH if:
1. They contain Ca and have oxygen ≥ 6.130, OR
2. They contain Hg but not Ca and have oxygen values of 4.081, 4.096, 4.097, 4.150, 4.162, or 4.220.

Materials with fractional stoichiometries for non-oxygen elements are LOW.
All other materials are LOW.

## Predictions for Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (Ca, oxygen=6.160 ≥ 6.130)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (Ca, oxygen=6.270 ≥ 6.130)
3. O4.047Cu1Ba2Hg1: LOW (Hg, no Ca, oxygen not in approved list)
4. O4.066Cu1Ba2Hg1: LOW (Hg, no Ca, oxygen not in approved list)
5. O4.160Cu1Ba2Hg1: LOW (Hg, no Ca, oxygen not in approved list)
6. O4.180Cu1Ba2Hg1: LOW (Hg, no Ca, oxygen not in approved list)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (fractional stoichiometries)
8. O8Ca0.850Cu2.200Ba2Tl1.800: LOW (fractional stoichiometries)

## Step D - Final Pick:
Among unmeasurable materials predicted HIGH: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1.
Both have 5 elements. Break tie by higher expected Tc:
- O6.160 between O6.140 (105K) and O6.190 (122K) → expected ~110-120K
- O6.270 between O6.240 (123K) and O6.300 (100K) → expected ~110K
O6.160 likely has higher expected Tc.

Final pick: O6.160Ca1Cu2Ba2Hg1

Budget after all measurements: used 45, remaining 0