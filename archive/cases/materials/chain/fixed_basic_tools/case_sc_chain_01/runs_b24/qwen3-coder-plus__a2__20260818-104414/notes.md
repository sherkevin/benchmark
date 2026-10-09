# Superconductor Tc Study Notes

## Step A - Series Peak
- Need to measure all compositions in data/measurable.txt to find the one with maximum Tc
- Starting with the seed material: O6.080Ca1Cu2Ba2Hg1

Measurement results:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAX)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (MAX)
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
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: (not measured due to budget limit)
- O7Cu3Y0.200Ba2La0.800: (not measured due to budget limit)

Step A Result:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have same Tc)
- Maximum Tc (T*): 127.0 K

## Step B - Filter Family B (contains Hg and has exactly 4 elements)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Materials containing Hg and having exactly 4 elements:
  - O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
  - O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
  - O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
  - O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
  - O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 K ✓
  - O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 K ✓
  - O4.047Cu1Ba2Hg1: 95.0 K >= 95.0 K ✓
  - O4.200Cu1Ba2Hg1: 94.2 K < 95.0 K (doesn't qualify)
  - O4.101Cu1Ba2Hg1: 93.4 K < 95.0 K (doesn't qualify)
  - O4.103Cu1Ba2Hg1: 92.5 K < 95.0 K (doesn't qualify)
  - O4.085Cu1Ba2Hg1: 92.0 K < 95.0 K (doesn't qualify)

Step B Result:
- Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}
- threshold1 = 95.0

## Step C - Rule Induction & Predictions for Unmeasurable Materials
First, let me analyze the high (>90K) vs low (≤90K) materials I measured:

High Tc (≥90K) materials:
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
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc (<90K) materials:
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K

Looking for patterns:
1. O6Cu1Ba2Hg1Tl1 (0.0 K) - has 5 elements: O, Cu, Ba, Hg, Tl
2. O6Cu1Cd0.250Ba2Tl1.750 (88.0 K) - has 5 elements: O, Cu, Cd, Ba, Tl

All the high Tc materials with 5 elements are in the Ca-Cu-Ba-Hg-O series (Tc range: 92.0-127.0K)
The low Tc material with 5 elements has Tl instead of Hg (0.0K) and another has Cd and Tl (88.0K)

Let me analyze more carefully:
- Materials with Tl: O6Cu1Ba2Hg1Tl1 (0.0 K), O6Cu1Cd0.250Ba2Tl1.750 (88.0 K), O10Ca2Cu3Ba2Tl2 (118.97 K) - mixed results
- Materials with Cd: O6Cu1Cd0.250Ba2Tl1.750 (88.0 K) - low Tc
- Materials with Hg: All the O4.x series and O6.xCa1Cu2Ba2Hg1 series - mostly high Tc except when combined with Tl and Cd

Wait, let me look at the specific elements combinations:
- High Tc materials with Hg but no Tl/Cd: All in the O4.xCu1Ba2Hg1 series and O6.xCa1Cu2Ba2Hg1 series
- Low Tc materials with Hg but with Tl/Cd: O6Cu1Ba2Hg1Tl1 (0.0 K) and O6Cu1Cd0.250Ba2Tl1.750 (88.0 K)

Rule hypothesis: Materials with Hg AND without Tl or Cd tend to have high Tc. Materials with Tl or Cd tend to have lower Tc.

Let me also consider oxygen content:
- O4.xCu1Ba2Hg1 series: O4.047 to O4.220 - high Tc range 92.0-98.0 K
- O6.xCa1Cu2Ba2Hg1 series: O6.080 to O6.310 - high Tc range 92.0-127.0 K

Looking at the unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Contains Ca, Cu, Ba, Hg, O - follows the high Tc series pattern → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - Contains Ca, Cu, Ba, Hg, O - follows the high Tc series pattern → HIGH
3. O4.180Cu1Ba2Hg1 - Contains Cu, Ba, Hg, O - follows the high Tc series pattern → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Ca, Cu, Ba, Tl, O (no Hg, has Tl) → LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Ca, Cu, Sr, Tl, Pb, O (no Hg, has Tl, Pb) → LOW
6. O8Cu3Y1Ba1.900La0.100 - Contains Cu, Y, Ba, La, O (no Hg) → Based on similar materials, likely LOW
7. O8Cu4Y1Ba1.870La0.130 - Contains Cu, Y, Ba, La, O (no Hg) → Based on similar materials, likely LOW

Refined rule: Materials are high Tc if they contain Hg AND don't contain Tl or Cd. Materials without Hg or with Tl/Cd tend to be low Tc.

Step C Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Contains Hg, no Tl/Cd → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - Contains Hg, no Tl/Cd → HIGH
3. O4.180Cu1Ba2Hg1 - Contains Hg, no Tl/Cd → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - No Hg, contains Tl → LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - No Hg, contains Tl, Pb → LOW
6. O8Cu3Y1Ba1.900La0.100 - No Hg → LOW
7. O8Cu4Y1Ba1.870La0.130 - No Hg → LOW

## Step D - Final Pick
Among the unmeasurable materials predicted HIGH in Step C, return the one with the FEWEST elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)

O4.180Cu1Ba2Hg1 has the fewest elements (4), so this is the answer for Step D.
