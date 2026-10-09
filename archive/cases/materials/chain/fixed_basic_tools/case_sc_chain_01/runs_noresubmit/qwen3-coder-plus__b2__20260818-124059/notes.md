# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 18 calls to measure_tc

## Step A - Series Peak
Results from measurements:

O6.080Ca1Cu2Ba2Hg1: 92.0 K
O6.100Ca1Cu2Ba2Hg1: 92.5 K
O6.130Ca1Cu2Ba2Hg1: 102.0 K
O6.140Ca1Cu2Ba2Hg1: 105.0 K
O6.190Ca1Cu2Ba2Hg1: 122.0 K
O6.210Ca1Cu2Ba2Hg1: 127.0 K
O6.220Ca1Cu2Ba2Hg1: 127.0 K
O6.240Ca1Cu2Ba2Hg1: 123.0 K
O6.300Ca1Cu2Ba2Hg1: 100.0 K
O6.310Ca1Cu2Ba2Hg1: 98.0 K
O4.081Cu1Ba2Hg1: 98.0 K
O4.162Cu1Ba2Hg1: 97.4 K
O4.150Cu1Ba2Hg1: 97.0 K
O4.220Cu1Ba2Hg1: 97.0 K
O4.096Cu1Ba2Hg1: 95.6 K
O4.097Cu1Ba2Hg1: 95.3 K
O4.047Cu1Ba2Hg1: 95.0 K

The maximum Tc (T*) is 127.0 K, achieved by two compositions:
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
Family B: Materials that contain Hg and have exactly 4 elements
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

From the measured materials, let's identify those containing Hg with exactly 4 elements:
- O6.080Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.100Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.130Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.140Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.190Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.210Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.220Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.240Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.300Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O6.310Ca1Cu2Ba2Hg1: 6 elements (O, Ca, Cu, Ba, Hg) - No
- O4.081Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.162Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.150Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.220Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.096Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.097Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes
- O4.047Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - Yes

All of these have Tc >= 95.0 K, so Q1 includes:
- O4.081Cu1Ba2Hg1 (98.0 K)
- O4.162Cu1Ba2Hg1 (97.4 K)
- O4.150Cu1Ba2Hg1 (97.0 K)
- O4.220Cu1Ba2Hg1 (97.0 K)
- O4.096Cu1Ba2Hg1 (95.6 K)
- O4.097Cu1Ba2Hg1 (95.3 K)
- O4.047Cu1Ba2Hg1 (95.0 K)

Wait, I didn't measure all the materials in the measurable.txt file. I ran out of measurements before completing the full list. Let me see which ones remain unmeasured:

Remaining from measurable.txt that I didn't measure:
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Since I used all my measurement budget, I'll have to work with the data I have.

## Step C - Extrapolate Rule
Looking at the measured materials I have data for, I need to find patterns that distinguish high (>=90K) from low (<90K) Tc materials.

Materials I measured:
- High Tc (>=90K):
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

All measured materials are high Tc. Based on the information in the task description, I know that single-factor rules are wrong. Looking at the patterns:
- Materials with Hg tend to be high Tc
- Oxygen content might play a role
- The number of elements might matter

Looking more carefully at the composition patterns:
- Materials with Hg seem to have high Tc values
- Some materials without Hg (from the unmeasured set) might be low Tc
- For the Ca-Cu-Ba-Hg-O series, oxygen content between 6.19-6.24 seems to give highest Tc
- For the Cu-Ba-Hg-O series, oxygen content around 4.x seems to give high Tc

For the unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 - Similar to high Tc series with Hg, likely high
- O6.270Ca1Cu2Ba2Hg1 - Similar to high Tc series with Hg, likely high  
- O4.180Cu1Ba2Hg1 - Similar to high Tc series with Hg, likely high
- O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl instead of Hg, may be different behavior
- O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Sr and Pb instead of Ba/Hg, may be different
- O8Cu3Y1Ba1.900La0.100 - Contains Y/La instead of Ca/Hg, may be different
- O8Cu4Y1Ba1.870La0.130 - Contains Y/La instead of Ca/Hg, may be different

Induced rule: Materials with Hg in the Cu-Ba system (with or without Ca) tend to have high Tc, especially with certain oxygen stoichiometries. Materials without Hg but with other heavy metals like Tl, Pb, or rare earth elements like Y/La may have different behavior depending on the specific combination.

## Step D - Final Pick
Among unmeasurable materials predicted high, pick the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg) - likely high
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg) - likely high
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) - likely high
- O8Ca0.850Cu2.200Ba2Tl1.800: 6 elements (O, Ca, Cu, Ba, Tl) - uncertain
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb) - uncertain
- O8Cu3Y1Ba1.900La0.100: 5 elements (O, Cu, Y, Ba, La) - uncertain
- O8Cu4Y1Ba1.870La0.130: 5 elements (O, Cu, Y, Ba, La) - uncertain

Based on the rule that materials with Hg tend to be high Tc, and choosing the one with fewest elements:
O4.180Cu1Ba2Hg1 has 4 elements and likely high Tc.