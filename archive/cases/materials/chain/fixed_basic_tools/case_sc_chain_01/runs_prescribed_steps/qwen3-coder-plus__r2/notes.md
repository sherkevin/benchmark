# Superconductor Tc Study Notes

## Step A - Series Peak
Starting measurements on the homologous doping series with composition O6.080Ca1Cu2Ba2Hg1 as the seed.

Measured the series members and found the composition with MAXIMUM Tc:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: null (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: null (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.180Cu1Ba2Hg1: null (unmeasurable)
- O4.047Cu1Ba2Hg1: null (unmeasurable)
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

T* (maximum Tc) = 127.0 K

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials that contain Hg and have exactly 4 elements:
- O4.081Cu1Ba2Hg1: 98.0 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.162Cu1Ba2Hg1: 97.4 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.150Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.220Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.096Cu1Ba2Hg1: 95.6 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.097Cu1Ba2Hg1: 95.3 K (4 elements: O, Cu, Ba, Hg) - meets Tc >= 95.0 requirement
- O4.200Cu1Ba2Hg1: 94.2 K (4 elements: O, Cu, Ba, Hg) - doesn't meet Tc >= 95.0 requirement
- O4.101Cu1Ba2Hg1: 93.4 K (4 elements: O, Cu, Ba, Hg) - doesn't meet Tc >= 95.0 requirement
- O4.103Cu1Ba2Hg1: 92.5 K (4 elements: O, Cu, Ba, Hg) - doesn't meet Tc >= 95.0 requirement
- O4.085Cu1Ba2Hg1: 92.0 K (4 elements: O, Cu, Ba, Hg) - doesn't meet Tc >= 95.0 requirement

So Q1 = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"]

## Step C - Induce Rule and Predict for Unmeasurable Materials

First, let me analyze all measured materials to identify the pattern that distinguishes high from low Tc materials:

High Tc materials (Tc >= 90 K):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (5 elements: O, Ca, Cu, Ba, Hg)
- O4.081Cu1Ba2Hg1: 98.0 K (4 elements: O, Cu, Ba, Hg)
- O4.162Cu1Ba2Hg1: 97.4 K (4 elements: O, Cu, Ba, Hg)
- O4.150Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg)
- O4.220Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg)
- O4.096Cu1Ba2Hg1: 95.6 K (4 elements: O, Cu, Ba, Hg)
- O4.097Cu1Ba2Hg1: 95.3 K (4 elements: O, Cu, Ba, Hg)
- O4.200Cu1Ba2Hg1: 94.2 K (4 elements: O, Cu, Ba, Hg)
- O4.101Cu1Ba2Hg1: 93.4 K (4 elements: O, Cu, Ba, Hg)
- O4.103Cu1Ba2Hg1: 92.5 K (4 elements: O, Cu, Ba, Hg)
- O4.085Cu1Ba2Hg1: 92.0 K (4 elements: O, Cu, Ba, Hg)
- O10Ca2Cu3Ba2Tl2: 118.97 K (5 elements: Ca, Cu, Ba, Tl, O)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (5 elements: Cu, Cd, Ba, Tl, O) - barely low
- O7Cu3Ba2La1: 89.6 K (4 elements: Cu, Ba, La, O) - low
- O7Cu3Y0.200Ba2La0.800: 81.0 K (4 elements: Cu, Ba, La, Y, O)

Low Tc materials (Tc < 90 K):
- O6Cu1Ba2Hg1Tl1: 0.0 K (5 elements: O, Cu, Ba, Hg, Tl)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (5 elements: Cu, Cd, Ba, Tl, O)
- O7Cu3Ba2La1: 89.6 K (4 elements: Cu, Ba, La, O)
- O7Cu3Y0.200Ba2La0.800: 81.0 K (5 elements: Cu, Ba, La, Y, O)

Looking at the patterns, I notice that:
1. Materials with Tl tend to have lower Tc (O6Cu1Ba2Hg1Tl1: 0.0 K, O6Cu1Cd0.250Ba2Tl1.750: 88.0 K, O10Ca2Cu3Ba2Tl2: 118.97 K)
2. Most Ca-containing materials have high Tc
3. Oxygen stoichiometry seems to play a role
4. Hg-only materials with 4 elements (O, Cu, Ba, Hg) show interesting patterns

For the unmeasurable materials, I need to analyze the pattern more carefully. Looking at the oxygen content in high-Tc Hg-Ba-Cu-O materials:
- O4.081Cu1Ba2Hg1: 98.0 K (high Tc)
- O4.162Cu1Ba2Hg1: 97.4 K (high Tc)
- O4.150Cu1Ba2Hg1: 97.0 K (high Tc)
- O4.220Cu1Ba2Hg1: 97.0 K (high Tc)
- O4.096Cu1Ba2Hg1: 95.6 K (high Tc)
- O4.097Cu1Ba2Hg1: 95.3 K (high Tc)
- O4.200Cu1Ba2Hg1: 94.2 K (high Tc)
- O4.101Cu1Ba2Hg1: 93.4 K (high Tc)
- O4.103Cu1Ba2Hg1: 92.5 K (high Tc)
- O4.085Cu1Ba2Hg1: 92.0 K (high Tc)

For the Ca-Ba-Cu-Hg-O series:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high Tc)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high Tc)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high Tc)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high Tc)
- O6.160Ca1Cu2Ba2Hg1: null (unmeasurable) - likely high based on trend
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high Tc)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high Tc)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high Tc)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high Tc)
- O6.270Ca1Cu2Ba2Hg1: null (unmeasurable) - likely high based on trend
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high Tc)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high Tc)

The pattern suggests that for Ca-Ba-Cu-Hg-O series, oxygen content around 6.19-6.22 gives maximum Tc, with both lower and higher oxygen content giving lower Tc values.

For the unmeasurable materials in the series:
- O6.160Ca1Cu2Ba2Hg1: likely high Tc (between 105.0 and 122.0 K based on neighboring compositions)
- O6.270Ca1Cu2Ba2Hg1: likely high Tc (between 123.0 and 100.0 K based on neighboring compositions)

For Hg-Ba-Cu-O series:
- O4.180Cu1Ba2Hg1: unmeasurable - looking at trend, O4.180 is between O4.162 (97.4 K) and O4.220 (97.0 K), so likely high Tc
- O4.047Cu1Ba2Hg1: unmeasurable - this is much lower oxygen than the lowest measured (O4.081: 92.0 K), so likely low Tc

Other unmeasurable materials:
- O4.066Cu1Ba2Hg1: between O4.081 (92.0 K) and O4.085 (92.0 K), likely high Tc
- O4.160Cu1Ba2Hg1: between O4.150 (97.0 K) and O4.162 (97.4 K), likely high Tc
- O8Ca0.850Cu2.200Ba2Tl1.800: contains Tl, which generally correlates with lower Tc (though O10Ca2Cu3Ba2Tl2 was 118.97 K), and has complex stoichiometry, likely low Tc
- O7Ca1Cu2Sr2Tl0.500Pb0.500: contains Sr and Pb instead of Ba, and Tl, likely low Tc
- O8Cu3Y1Ba1.900La0.100: contains Y and La instead of Ca, likely low Tc
- O8Cu4Y1Ba1.870La0.130: contains Y and La instead of Ca, likely low Tc

Induced rule: High Tc materials tend to have Ca, Ba, Cu, Hg with appropriate oxygen stoichiometry (around 6.19-6.22 for Ca-Ba-Cu-Hg-O series and around 4.08-4.22 for Hg-Ba-Cu-O series). Materials with Tl, La, Y instead of Ca, or unusual combinations of elements tend to have lower Tc.

## Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: high (based on interpolation between O6.140: 105.0K and O6.190: 122.0K)
- O6.270Ca1Cu2Ba2Hg1: high (based on interpolation between O6.240: 123.0K and O6.300: 100.0K)  
- O4.180Cu1Ba2Hg1: high (interpolated between O4.162: 97.4K and O4.220: 97.0K)
- O4.047Cu1Ba2Hg1: low (lower oxygen than baseline O4.081: 92.0K, likely below 90K)
- O4.066Cu1Ba2Hg1: high (between O4.081: 92.0K and O4.085: 92.0K)
- O4.160Cu1Ba2Hg1: high (between O4.150: 97.0K and O4.162: 97.4K)
- O8Ca0.850Cu2.200Ba2Tl1.800: low (contains Tl which generally correlates with lower Tc)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (contains Sr, Pb, Tl instead of usual elements)
- O8Cu3Y1Ba1.900La0.100: low (contains Y, La instead of Ca)
- O8Cu4Y1Ba1.870La0.130: low (contains Y, La instead of Ca)

## Step D - Final Pick
Among the unmeasurable materials predicted to be high, I need to choose the one with the FEWEST elements (breaking ties by higher expected Tc):

Predicted high unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
- O4.160Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)

The materials with the fewest elements (4) are: O4.180Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, and O4.160Cu1Ba2Hg1.

Breaking ties by expected Tc (based on interpolation):
- O4.180Cu1Ba2Hg1: interpolated between O4.162 (97.4K) and O4.220 (97.0K) ≈ 97.2K
- O4.066Cu1Ba2Hg1: between O4.081 (92.0K) and O4.085 (92.0K) ≈ 92.0K
- O4.160Cu1Ba2Hg1: between O4.150 (97.0K) and O4.162 (97.4K) ≈ 97.2K

Between O4.180Cu1Ba2Hg1 and O4.160Cu1Ba2Hg1 (both ~97.2K), I'll pick O4.180Cu1Ba2Hg1 as the final answer since it was closer to the optimal range in the measured series.