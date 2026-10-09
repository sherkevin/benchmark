# Superconductor Tc Study Notes

## Initial Setup
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- "High" Tc: ≥ 90 K, otherwise "low"
- Search_db returns max 4 results per page, costs 1 unit per page
- Measure_tc costs 1 unit per measurement

## Step A - Series Peak
Need to find homologous doping series members (same element set, varying oxygen).
Goal: Find composition with maximum Tc (T*)

Approach:
1. Search for materials containing Hg, Cu, Ba, Ca (elements from seed)
2. Identify series by analyzing oxygen variations
3. Measure Tc for all series members to find peak

## Budget Planning
Total budget: 45 units
Need to allocate wisely between searches and measurements.

## Step A Progress
Found homologous series members (Ca1Cu2Ba2Hg1 with varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1 (seed)
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

Total series members: 12 materials to measure.

Also found other Ca-containing materials:
- O10Ca2Cu3Ba2Tl2
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Ca0.850Cu2.200Ba2Tl1.800

## Step A Results
Measured Tc for homologous series members (Ca1Cu2Ba2Hg1 with varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Peak Tc (T*): 127.0 K at compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
I'll use O6.210Ca1Cu2Ba2Hg1 as the peak composition (first occurrence of maximum).

## Step B Analysis
T* = 127.0 K
threshold1 = T* - 32 = 95.0 K

Need to find all Hg-containing materials with exactly 4 elements and Tc ≥ 95.0 K.

Hg-containing materials with exactly 4 elements (O4.xxxCu1Ba2Hg1):
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

Total: 14 materials to measure for Step B.

Unmeasurable materials found so far:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

## Step B Results
Threshold: 95.0 K

All 4-element Hg materials measured:
1. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)
2. O4.066Cu1Ba2Hg1: Unmeasurable (no sample)
3. O4.081Cu1Ba2Hg1: 98.0 K ✓ QUALIFIES
4. O4.085Cu1Ba2Hg1: 92.0 K ✗
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ QUALIFIES
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ QUALIFIES
7. O4.101Cu1Ba2Hg1: 93.4 K ✗
8. O4.103Cu1Ba2Hg1: 92.5 K ✗
9. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
10. O4.160Cu1Ba2Hg1: Unmeasurable (no sample)
11. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES
12. O4.180Cu1Ba2Hg1: Unmeasurable (no sample)
13. O4.200Cu1Ba2Hg1: 94.2 K ✗
14. O4.220Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES

Q1 (qualifying materials with Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.096Cu1Ba2Hg1 (95.6 K)
3. O4.097Cu1Ba2Hg1 (95.3 K)
4. O4.150Cu1Ba2Hg1 (97.0 K)
5. O4.162Cu1Ba2Hg1 (97.4 K)
6. O4.220Cu1Ba2Hg1 (97.0 K)

## Additional Measurements
Measured other Ca-containing materials:
- O10Ca2Cu3Ba2Tl2: 118.97 K (high)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Unmeasurable
- O8Ca0.850Cu2.200Ba2Tl1.800: Unmeasurable

## Step C - Rule Induction
Need to induce a rule from measurable materials to predict high/low for unmeasurable materials.

All unmeasurable materials (8 total):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.066Cu1Ba2Hg1
5. O4.160Cu1Ba2Hg1
6. O4.180Cu1Ba2Hg1
7. O7Ca1Cu2Sr2Tl0.500Pb0.500
8. O8Ca0.850Cu2.200Ba2Tl1.800

Need to predict high/low for all 8.

## Step D - Final Pick
Among unmeasurable materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.160Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements)

Fewest elements: O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1 (both 4 elements)
Break tie by higher expected Tc: O4.160Cu1Ba2Hg1 (between O4.150@97.0K and O4.162@97.4K, expected ~97.2K) vs O4.180Cu1Ba2Hg1 (between O4.162@97.4K and O4.200@94.2K, expected ~95.8K)
So O4.160Cu1Ba2Hg1 has higher expected Tc.

Final pick: O4.160Cu1Ba2Hg1

## Rule Induced
Rule: Materials containing Ca are high if oxygen ≥ 6.130. Materials not containing Ca are high if oxygen ≥ 4.080 and (oxygen fractional part modulo 0.020 ≠ 0.005 or oxygen = 4.220).

Budget used: 45 units (all budget used)
Budget remaining: 0 units