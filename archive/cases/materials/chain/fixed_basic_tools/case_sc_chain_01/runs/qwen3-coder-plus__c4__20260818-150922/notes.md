# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 18 calls to measure_tc

## Step A - Series Peak
Measuring compositions in measurable.txt to find the one with maximum Tc.

### Measurements taken:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.047Cu1Ba2Hg1: 95.0 K

Maximum Tc (T*) = 127.0 K, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
Family B contains materials with Hg and exactly 4 elements.
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Materials with Hg and exactly 4 elements from my measurements:
- O4.081Cu1Ba2Hg1: 98.0 K (≥ 95.0 K, qualifies)
- O4.162Cu1Ba2Hg1: 97.4 K (≥ 95.0 K, qualifies)
- O4.150Cu1Ba2Hg1: 97.0 K (≥ 95.0 K, qualifies)
- O4.220Cu1Ba2Hg1: 97.0 K (≥ 95.0 K, qualifies)
- O4.096Cu1Ba2Hg1: 95.6 K (≥ 95.0 K, qualifies)
- O4.097Cu1Ba2Hg1: 95.3 K (≥ 95.0 K, qualifies)
- O4.047Cu1Ba2Hg1: 95.0 K (≥ 95.0 K, qualifies)

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Extrapolate
For the unmeasurable materials, I need to induce a rule to separate high from low Tc.
Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Based on the measurable data, I notice:
1. The Ca-Cu-Ba-Hg-O series with around 6 oxygens have the highest Tc values
2. The Cu-Ba-Hg-O series with around 4 oxygens have moderate Tc values (95-98 K)
3. Tc seems to peak at certain oxygen stoichiometries

Looking at the oxygen content relationship with Tc in the Ca-Cu-Ba-Hg-O series:
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.190: 122.0 K
- O6.210: 127.0 K
- O6.220: 127.0 K
- O6.240: 123.0 K
- O6.300: 100.0 K
- O6.310: 98.0 K

It appears that Tc peaks around O6.210-O6.220 and decreases on either side.

## Step C - Extrapolate continued
Induced rule: Materials with the combination of Ca, Cu, Ba, and Hg elements with oxygen content around 6.2 (specifically 6.19-6.24) tend to have the highest Tc values (>100K). Materials with Cu, Ba, Hg but lower oxygen content (~4) have moderate Tc values (95-98K). The presence of certain elements like Ca vs Y/La, or Ba vs Sr, or Hg vs Tl/Pb seem to affect the Tc significantly.

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: HIGH (likely ~110-115K based on interpolation in the high-Tc series)
- O6.270Ca1Cu2Ba2Hg1: HIGH (likely ~110-120K based on interpolation in the high-Tc series)
- O4.180Cu1Ba2Hg1: HIGH (similar to other 4.xO Cu-Ba-Hg compounds at ~97K)
- O8Ca0.850Cu2.200Ba2Tl1.800: LOW (substituted Tl for Hg, different stoichiometry)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (Sr instead of Ba, Tl/Pb instead of Hg)
- O8Cu3Y1Ba1.900La0.100: LOW (Y/La instead of Ca, different cation mix)
- O8Cu4Y1Ba1.870La0.130: LOW (Y/La instead of Ca, different cation mix)

## Step D - Final Pick
From the predicted HIGH materials: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1
Counting elements in each:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)

The one with the fewest elements is O4.180Cu1Ba2Hg1 with 4 elements.