# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (shared between search_db and measure_tc)
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
Need to find the homologous doping series for the seed material (same elements, varying oxygen).
The seed material is O6.080Ca1Cu2Ba2Hg1

Materials found in the series:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Peak Tc (T*) = 127.0 K
Peak compositions: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0
Need to find materials containing Hg with exactly 4 elements that have Tc >= 95.0 K

4-element materials containing Hg:
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (>= 95.0, qualifies)
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95.0)
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (>= 95.0, qualifies)
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (>= 95.0, qualifies)
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95.0)
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (< 95.0)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0, qualifies)
- O4.160Cu1Ba2Hg1: No sample available
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (>= 95.0, qualifies)
- O4.180Cu1Ba2Hg1: No sample available
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0)
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0, qualifies)

Q1 (materials with Hg, exactly 4 elements, Tc >= 95.0):
- O4.081Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

## Step C - Extrapolate
Now I need to predict high/low for all unmeasurable materials I found during my searches.
Unmeasurable materials I found:

Based on the pattern observed, the Tc tends to peak at certain oxygen content ranges:
- For Ca1Cu2Ba2Hg1: peak Tc around O6.19-O6.24 range, lower Tc at extremes
- For Cu1Ba2Hg1: high Tc materials have oxygen content around specific ranges (4.08-4.10, 4.15-4.16, 4.22)

Predictions:
- O6.160Ca1Cu2Ba2Hg1: Predicted HIGH (oxygen content 6.160 is near the high Tc range)
- O6.270Ca1Cu2Ba2Hg1: Predicted LOW (oxygen content 6.270 is moving away from peak toward lower Tc range)
- O4.047Cu1Ba2Hg1: Predicted LOW (oxygen content 4.047 is below the high Tc range)
- O4.066Cu1Ba2Hg1: Predicted LOW (oxygen content 4.066 is below the high Tc range)
- O4.160Cu1Ba2Hg1: Predicted HIGH (oxygen content 4.160 is in the high Tc range)
- O4.180Cu1Ba2Hg1: Predicted HIGH (oxygen content 4.180 is near the high Tc range)

Rule: Tc appears to depend on the oxygen stoichiometry, with optimal superconductivity occurring at specific oxygen content ranges for each material family. Single-factor rules based only on oxygen content aren't always accurate, but oxygen stoichiometry is a key factor affecting Tc.

## Step D - Final Pick
From the unmeasurable materials predicted as HIGH:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O4.160Cu1Ba2Hg1 (4 elements) - fewer elements
- O4.180Cu1Ba2Hg1 (4 elements) - fewer elements

Among the materials predicted as HIGH with the fewest elements (4), I need to choose the one with higher expected Tc. Between O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1, since both have 4 elements, I'll choose based on proximity to known high Tc values in the same family.

For Cu1Ba2Hg1 family:
- O4.150Cu1Ba2Hg1: Tc = 97.0 K
- O4.160Cu1Ba2Hg1: Predicted HIGH
- O4.162Cu1Ba2Hg1: Tc = 97.4 K
- O4.180Cu1Ba2Hg1: Predicted HIGH
- O4.200Cu1Ba2Hg1: Tc = 94.2 K

Since O4.162Cu1Ba2Hg1 has Tc = 97.4 K, O4.160 is very close to this high-Tc composition, so it likely has a higher Tc than O4.180Cu1Ba2Hg1.

Final answer: O4.160Cu1Ba2Hg1