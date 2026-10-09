# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 18 measure_tc calls

## Step A - Series Peak
Results of measurements:
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
- O4.200Cu1Ba2Hg1: 94.2 K

Maximum Tc found: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

So T* = 127.0 K, and the peak composition is O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg and having exactly 4 elements with Tc >= 95.0 K (Q1):
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (High)
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (High)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (High)
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (High)
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (High)
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (High)
- O4.047Cu1Ba2Hg1: Tc = 95.0 K (High)

## Step C - Extrapolate Rule
From the measured compositions, I need to determine a rule to classify high vs low Tc materials.

Analyzing the measured compositions and their Tc values:
Series O6.xCa1Cu2Ba2Hg1 (5 elements):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (Low)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (Low)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (High)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (High)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (High)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (High)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (High)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (High)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (Low)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (High)

Series O4.xCu1Ba2Hg1 (4 elements):
- O4.081Cu1Ba2Hg1: 98.0 K (High)
- O4.162Cu1Ba2Hg1: 97.4 K (High)
- O4.150Cu1Ba2Hg1: 97.0 K (High)
- O4.220Cu1Ba2Hg1: 97.0 K (High)
- O4.096Cu1Ba2Hg1: 95.6 K (High)
- O4.097Cu1Ba2Hg1: 95.3 K (High)
- O4.047Cu1Ba2Hg1: 95.0 K (High)
- O4.200Cu1Ba2Hg1: 94.2 K (Low)

Other measurable compositions (not measured due to budget):
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Looking at the pattern, I can observe:
1. In the O6.xCa1Cu2Ba2Hg1 series, there's a peak around O6.210-O6.220 with Tc=127.0K, then it drops
2. Oxygen content seems to play a role in Tc
3. The O4.xCu1Ba2Hg1 series generally shows high Tc values (except the highest oxygen content O4.200Cu1Ba2Hg1)
4. Materials with fewer elements tend to have higher Tc when other conditions are similar

Rule hypothesis: Materials with Hg and 4 or fewer elements are likely to have high Tc, especially when oxygen content is moderate.

For the unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: Part of the same series as seed, 5 elements, oxygen 6.160 - likely HIGH (between O6.140 and O6.190)
- O6.270Ca1Cu2Ba2Hg1: Part of the same series as seed, 5 elements, oxygen 6.270 - likely HIGH (similar to O6.240 which is 123K)
- O4.180Cu1Ba2Hg1: 4 elements, oxygen 4.180 - likely HIGH (between O4.162 and O4.200, probably between 96K-97K)
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl, 5 elements - likely LOW (based on complexity)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Sr and Pb, 6 elements - likely LOW (too many elements)
- O8Cu3Y1Ba1.900La0.100: Contains Y and La, 5 elements - likely LOW (different cation mix)
- O8Cu4Y1Ba1.870La0.130: Contains Y and La, 5 elements - likely LOW (different cation mix)