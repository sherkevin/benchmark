# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- Definition: High Tc ≥ 90 K, Low Tc < 90 K

## Step A - Series Peak
Need to measure all compositions in data/measurable.txt and find the one with MAXIMUM Tc.
Results so far:
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

Maximum Tc (T*) = 127.0 K, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Find materials containing Hg with exactly 4 elements where Tc ≥ 95.0 K

From the measured compositions containing Hg with 4 elements:
- O4.081Cu1Ba2Hg1: 98.0 K ≥ 95.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K ≥ 95.0 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K ≥ 95.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K ≥ 95.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K ≥ 95.0 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K ≥ 95.0 K ✓
- O4.047Cu1Ba2Hg1: 95.0 K ≥ 95.0 K ✓
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0 K ✗

Note: Could not measure O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1 due to budget constraints.

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Extrapolate
Create a rule based on measurable materials to predict high/low for unmeasurable materials

For the remaining measurable materials that I couldn't measure, I'll analyze the patterns from measured materials to induce a rule:
- Materials with Ca, Cu, Ba, Hg seem to have higher Tc values (up to 127K)
- Materials with just Cu, Ba, Hg have moderate Tc values around 94-98K
- Oxygen content seems to affect Tc significantly in Ca-Cu-Ba-Hg series
- Other compositions (with Tl, La, Cd, Sr, Y, Pb) were not measured due to budget

Looking at the measured data, the highest Tc values come from Ca1Cu2Ba2Hg1 compositions with specific oxygen content (around 6.19-6.22).

For unmeasurable materials, I'll analyze their characteristics:
- O6.160Ca1Cu2Ba2Hg1: Similar to high Tc series, likely HIGH
- O6.270Ca1Cu2Ba2Hg1: Similar to high Tc series, likely HIGH  
- O4.180Cu1Ba2Hg1: Similar to the series O4.081Cu1Ba2Hg1 (98.0K), O4.162Cu1Ba2Hg1 (97.4K), etc. - likely HIGH (interpolated ~97K)
- O8Ca0.850Cu2.200Ba2Tl1.800: Different composition with Tl, likely LOW
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Different composition with Sr and Pb, likely LOW
- O8Cu3Y1Ba1.900La0.100: Different composition with Y and La, likely LOW
- O8Cu4Y1Ba1.870La0.130: Different composition with Y and La, likely LOW

## Step D - Final Pick
Among unmeasurable materials predicted as high, return the one with fewest elements
Based on corrected predictions, the high Tc materials are:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.180Cu1Ba2Hg1 (4 elements) <- FEWEST ELEMENTS

So the final pick is O4.180Cu1Ba2Hg1.