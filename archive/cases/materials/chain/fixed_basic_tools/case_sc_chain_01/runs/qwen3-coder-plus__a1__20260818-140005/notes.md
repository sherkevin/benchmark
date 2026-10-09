# Superconductor Tc Study Notes

## Step A - Series Peak
Seed composition: O6.080Ca1Cu2Ba2Hg1

Need to measure the series members in measurable.txt to find the composition with maximum Tc.

Series members with same element set (Ca, Cu, Ba, Hg) and varying oxygen:
- O6.080Ca1Cu2Ba2Hg1 (seed): 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Not measured (budget exhausted)

Other series members with same element set (Cu, Ba, Hg) and varying oxygen:
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.047Cu1Ba2Hg1: 95.0 K
- O4.200Cu1Ba2Hg1: 94.2 K

Maximum Tc (T*) = 127.0 K for composition O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
Family B: Contains Hg and has exactly 4 elements
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg with exactly 4 elements that I was able to measure:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 K ✓
- O4.047Cu1Ba2Hg1: 95.0 K >= 95.0 K ✓ (exactly at threshold)
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0 K ✗

Note: Could not measure other compositions like O6Cu1Ba2Hg1Tl1 due to budget exhaustion.
These had more than 4 elements anyway.

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Extrapolate
For the unmeasurable materials, I need to induce a rule based on the measurable ones to predict high/low Tc.

Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

From my measured data, I can analyze patterns:
- High Tc materials tend to have optimal oxygen doping levels
- Based on the Ca-Cu-Ba-Hg series: O6.190Ca1Cu2Ba2Hg1 (122.0K), O6.210Ca1Cu2Ba2Hg1 (127.0K), O6.220Ca1Cu2Ba2Hg1 (127.0K) showed highest Tc values
- The optimal oxygen level seems to be around 6.21-6.22 for the Ca-Cu-Ba-Hg system
- For the Cu-Ba-Hg system: O4.081Cu1Ba2Hg1 had the highest Tc at 98.0K

Rule: Materials with oxygen content near optimal levels for their specific elemental composition will have high Tc. 
For Ca-Cu-Ba-Hg system: O around 6.19-6.24 gives high Tc
For Cu-Ba-Hg system: O around 4.04-4.09 gives high Tc
Also, having fewer additional elements generally helps maintain higher Tc values.

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: high (close to optimal range for this system)
- O6.270Ca1Cu2Ba2Hg1: low (too much oxygen, likely past optimal)
- O4.180Cu1Ba2Hg1: low (higher than optimal O for this system)
- O8Ca0.850Cu2.200Ba2Tl1.800: low (too many elements, complex structure)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (too many elements, complex structure)
- O8Cu3Y1Ba1.900La0.100: low (too many elements, complex structure)
- O8Cu4Y1Ba1.870La0.130: low (too many elements, complex structure)