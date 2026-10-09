# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 34 measure_tc calls
- Goal: Find peak Tc in homologous series (Step A), then identify qualifying materials (Step B), predict unmeasurable materials (Step C), and select final material (Step D)

## Step A - Find Peak Tc in Series - COMPLETED
The homologous doping series (same element set, varying oxygen) appears to be:
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

**Results:**
- Maximum Tc (T*) = 127.0 K
- Peak composition = O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127.0 K)
- Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Measurement Plan - UPDATED
1. ✅ Measure all 10 materials in the homologous series (Step A) - 10 measurements DONE
2. ✅ Identify threshold1 = T* - 32 = 95.0 K DONE
3. For Step B: Need to check all materials that contain Hg and have exactly 4 elements
   - From measurable.txt, identified 11 candidates with Hg and exactly 4 elements
   - Need to measure Tc for all 11 candidates to check if Tc >= threshold1 (95.0 K)
   - Budget used: 10/34, remaining: 24 measurements

## Step B Results (contain Hg, exactly 4 elements):
**Threshold1 = 95.0 K**
1. O4.081Cu1Ba2Hg1 - Tc = 98.0 K ✅ QUALIFIES
2. O4.162Cu1Ba2Hg1 - Tc = 97.4 K ✅ QUALIFIES
3. O4.150Cu1Ba2Hg1 - Tc = 97.0 K ✅ QUALIFIES
4. O4.220Cu1Ba2Hg1 - Tc = 97.0 K ✅ QUALIFIES
5. O4.096Cu1Ba2Hg1 - Tc = 95.6 K ✅ QUALIFIES
6. O4.097Cu1Ba2Hg1 - Tc = 95.3 K ✅ QUALIFIES
7. O4.047Cu1Ba2Hg1 - Tc = 95.0 K ✅ QUALIFIES
8. O4.200Cu1Ba2Hg1 - Tc = 94.2 K ❌ DOES NOT QUALIFY
9. O4.101Cu1Ba2Hg1 - Tc = 93.4 K ❌ DOES NOT QUALIFY
10. O4.103Cu1Ba2Hg1 - Tc = 92.5 K ❌ DOES NOT QUALIFY
11. O4.085Cu1Ba2Hg1 - Tc = 92.0 K ❌ DOES NOT QUALIFY

**Qualifying Set Q1:** O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1

**Budget used:** 21/34, **remaining:** 13 measurements

## All Measurements (Step C Data Collection):
**High Tc (≥ 90 K):**
1. O6.080Ca1Cu2Ba2Hg1 - 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1 - 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - 105.0 K
5. O6.190Ca1Cu2Ba2Hg1 - 122.0 K
6. O6.210Ca1Cu2Ba2Hg1 - 127.0 K
7. O6.220Ca1Cu2Ba2Hg1 - 127.0 K
8. O6.240Ca1Cu2Ba2Hg1 - 123.0 K
9. O6.300Ca1Cu2Ba2Hg1 - 100.0 K
10. O6.310Ca1Cu2Ba2Hg1 - 98.0 K
11. O4.081Cu1Ba2Hg1 - 98.0 K
12. O4.162Cu1Ba2Hg1 - 97.4 K
13. O4.150Cu1Ba2Hg1 - 97.0 K
14. O4.220Cu1Ba2Hg1 - 97.0 K
15. O4.096Cu1Ba2Hg1 - 95.6 K
16. O4.097Cu1Ba2Hg1 - 95.3 K
17. O4.047Cu1Ba2Hg1 - 95.0 K
18. O4.200Cu1Ba2Hg1 - 94.2 K
19. O4.101Cu1Ba2Hg1 - 93.4 K
20. O4.103Cu1Ba2Hg1 - 92.5 K
21. O4.085Cu1Ba2Hg1 - 92.0 K
22. O10Ca2Cu3Ba2Tl2 - 118.97 K

**Low Tc (< 90 K):**
1. O6Cu1Ba2Hg1Tl1 - 0.0 K
2. O6Cu1Cd0.250Ba2Tl1.750 - 88.0 K
3. O7Cu3Ba2La1 - 89.6 K
4. O7Cu3Y0.200Ba2La0.800 - 81.0 K

**Budget used:** 26/34, **remaining:** 8 measurements

## Step C - Rule Induction and Predictions

**Induced Rule:** A material has HIGH Tc (≥ 90 K) if it satisfies ALL of the following:
1. Contains Cu
2. Contains Ba
3. Contains EITHER:
   - Hg (but NOT Tl) OR
   - Tl WITH Ca

Otherwise, the material has LOW Tc (< 90 K).

**Rule Verification:** This rule correctly classifies all 26 measured materials.

**Predictions for Unmeasurable Materials:**
1. O6.160Ca1Cu2Ba2Hg1: Contains Cu, Ba, Hg (no Tl) → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Cu, Ba, Hg (no Tl) → HIGH
3. O4.180Cu1Ba2Hg1: Contains Cu, Ba, Hg (no Tl) → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Cu, Ba, Tl with Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Cu, Tl with Ca, but has Sr instead of Ba → LOW
6. O8Cu3Y1Ba1.900La0.100: Contains Cu, Ba, but no Hg or Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: Contains Cu, Ba, but no Hg or Tl → LOW

**High Tc predictions:** O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800

**Low Tc predictions:** O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130

## Step D - Final Pick
Among unmeasurable materials predicted HIGH:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements  
3. O4.180Cu1Ba2Hg1 - 4 elements (FEWEST elements)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements

**Final pick:** O4.180Cu1Ba2Hg1 (has fewest elements - 4)

**Final budget used:** 26/34 measurements
**Remaining budget:** 8 measurements unused