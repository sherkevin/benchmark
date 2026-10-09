# Superconductor Tc Study Notes

## Step A - Series Peak Analysis
**Goal**: Find maximum Tc in homologous doping series (same element set, varying oxygen).

**Materials measured for Step A**:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1 - NO SAMPLE (also in unmeasurable.txt)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1 - NO SAMPLE (also in unmeasurable.txt)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K ✓

**Maximum Tc (T*)**: 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

**Measurements used**: 10 successful measurements, 2 failed (no sample)

**Current budget**: 10/34 used

## Step B - Filter Family B
**Goal**: Among materials that contain Hg and have exactly 4 elements, report all with Tc >= threshold1 (T* - 32).

**Threshold1**: T* - 32 = 127.0 - 32 = 95.0 K

**Materials that contain Hg and have exactly 4 elements** (from measurable.txt analysis):
1. O4.081Cu1Ba2Hg1 - line 13 ✓
2. O4.162Cu1Ba2Hg1 - line 14 ✓
3. O4.150Cu1Ba2Hg1 - line 15 ✓
4. O4.220Cu1Ba2Hg1 - line 16 ✓
5. O4.096Cu1Ba2Hg1 - line 17 ✓
6. O4.097Cu1Ba2Hg1 - line 18 ✓
7. O4.180Cu1Ba2Hg1 - line 19 ✓
8. O4.047Cu1Ba2Hg1 - line 20 ✓
9. O4.200Cu1Ba2Hg1 - line 21 ✓
10. O4.101Cu1Ba2Hg1 - line 22 ✓
11. O4.103Cu1Ba2Hg1 - line 23 ✓
12. O4.085Cu1Ba2Hg1 - line 24 ✓

All these have pattern O[number]Cu1Ba2Hg1 - 4 elements: O, Cu, Ba, Hg

**Other Hg-containing materials** (but have ≠ 4 elements):
- Series materials (O6.xxxCa1Cu2Ba2Hg1) - 5 elements
- O6Cu1Ba2Hg1Tl1 - 5 elements
- O10Ca2Cu3Ba2Tl2 - No Hg
- O6Cu1Cd0.250Ba2Tl1.750 - No Hg
- O7Cu3Ba2La1 - No Hg
- O7Cu3Y0.200Ba2La0.800 - No Hg

**Need to measure**: 12 materials with Hg and exactly 4 elements

## Step C - Extrapolation Rule
**Goal**: Induce a rule separating high (Tc >= 90K) from low Tc materials based on measurable materials, then predict for unmeasurable materials.

**Unmeasurable materials** (10 total):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O4.047Cu1Ba2Hg1
5. O4.066Cu1Ba2Hg1
6. O4.160Cu1Ba2Hg1
7. O8Ca0.850Cu2.200Ba2Tl1.800
8. O7Ca1Cu2Sr2Tl0.500Pb0.500
9. O8Cu3Y1Ba1.900La0.100
10. O8Cu4Y1Ba1.870La0.130

## Step D - Final Pick
**Goal**: Among unmeasurable materials predicted HIGH in Step C, pick one with fewest elements (ties broken by higher expected Tc).

## Measurement Strategy
1. First measure all 12 series materials for Step A
2. Identify materials for Step B filtering
3. Measure remaining materials needed for rule induction
4. Keep track of budget (34 measurements total)