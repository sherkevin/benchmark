# Superconductor Tc Study Notes

## Task Overview
I need to:
1. Measure all materials in `data/measurable.txt` (27 compositions) to find maximum Tc (T*)
2. Find all materials containing Hg with exactly 4 elements that have Tc >= (T* - 32)
3. Induce a rule to predict high/low Tc for unmeasurable materials
4. Select the unmeasurable material with fewest elements among predicted high Tc

Measurement budget: 18 calls to `measure_tc`. Need to be strategic.

## Step A Analysis
The seed material `O6.080Ca1Cu2Ba2Hg1` belongs to a homologous doping series with varying oxygen. Looking at measurable.txt:
- Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 series (10 compositions)
- Lines 11-21: O4.xxxCu1Ba2Hg1 series (11 compositions) 
- Lines 22-27: Various other compositions (6 compositions)

Total: 27 measurable compositions, but only 18 measurement calls available. Need to prioritize.

## Strategy
1. Start by measuring the O6.xxxCa1Cu2Ba2Hg1 series (seed's homologous series)
2. Then measure other materials
3. Focus on finding maximum Tc efficiently

Let's begin measurements.

## Step A Measurements (Homologous Series)

Measurements completed for O6.xxxCa1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (current max)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (tied max)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Current maximum Tc (T*): 127.0 K
Budget used:不计 10/18 remaining

## Additional Measurements (Other Materials)

11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O10Ca2Cu3Ba2Tl2: 118.97 K
14. O6Cu1Ba2Hg1Tl1: 0.0 K
15. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
16. O7Cu3Ba2La1: 89.6 K
17. O7Cu3Y0.200Ba2La0.800: 81.0 K
18. O4.200Cu1Ba2Hg1: 94.2 K

Maximum Tc (T*): 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Need to choose one as "peak" - O6.210Ca1Cu2Ba2Hg1 appears first.

Budget fully used: 18/18 measurements completed.

## Step B Analysis

Threshold1 = T* - 32 = 127 - 32 = 95 K

Materials containing Hg with exactly 4 elements (from measurements):
1. O4.081Cu1Ba2Hg1: 98.0 K >= 95 K ✓
2. O4.162Cu1Ba2Hg1: 97.4 K >= 95 K ✓
3. O4.200Cu1Ba2Hg1: 94.2 K < 95 K ✗

Qualifying set Q1: ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1"]

Note: There are other O4.xxxCu1Ba2Hg1 materials in measurable.txt that I didn't measure due to budget constraints. Based on trend, O4.047, O4.085, O4.096, O4.097, O4.101, O4.103 might also qualify, and O4.150 might be borderline, but I don't have actual measurements.

## Step C Analysis - Inducing Prediction Rule

Measured Tc data summary:
- 14 materials have HIGH Tc (>= 90 K)
- 4 materials have LOW Tc (< 90 K)

Low Tc materials:
1. O6Cu1Ba2Hg1Tl1: 0.0 K (Hg + Tl)
2. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (Cd + Tl) 
3. O7Cu3Ba2La1: 89.6 K (La)
4. O7Cu3Y0.200Ba2La0.800: 81.0 K (Y + La)

High Tc materials include:
- All O6.xxxCa1Cu2Ba2Hg1 (Hg but no Tl)
- All O4.xxxCu1Ba2Hg1 (Hg but no Tl)
- O10Ca2Cu3Ba2Tl2 (Tl but no group 12 elements)

Induced rule: Material is LOW Tc if it contains EITHER:
1. Both a group 12 element (Zn, Cd, Hg) AND Tl (group 13), OR
2. La (lanthanide)

Otherwise HIGH Tc.

Rationale:
- Hg+Tl combination gives very low Tc (0 K)
- Cd+Tl gives low Tc (88 K)
- La-containing materials have borderline/low Tc (89.6 K, 81 K)
- Tl alone (without group 12) gives high Tc (O10Ca2Cu3Ba2Tl2: 118.97 K)
- Hg alone (without Tl) gives high Tc (all O6/O4 series)

Rule validation against measured data:
- All 4 low Tc materials fit rule ✓
- All 14 high Tc materials fit rule ✓

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (Hg but no Tl, no La)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (Hg but no Tl, no La)
3. O4.180Cu1Ba2Hg1: HIGH (Hg but no Tl, no La)
4. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (Tl but no group12, no La)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (Tl but no group12, no La)
6. O8Cu3Y1Ba1.900La0.100: LOW (contains La)
7. O8Cu4Y1Ba1.870La0.130: LOW (contains La)

High predicted: 5 materials
Low predicted: 2 materials

## Step D Analysis

Among unmeasurable materials predicted HIGH:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
3. O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

O4.180Cu1Ba2Hg1 has the fewest elements (4). No tie to break.

Final pick: O4.180Cu1Ba2Hg1

## Summary
Answer.json created with:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc=127.0 K)
- Threshold: 95.0 K
- Qualifying materials: O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1
- Predictions for all 7 unmeasurable materials
- Final selection: O4.180Cu1Ba2Hg1
- Induced rule: Group12+Tl or La causes low Tc