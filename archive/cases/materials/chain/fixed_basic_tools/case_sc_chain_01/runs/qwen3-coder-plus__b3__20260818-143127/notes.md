# Superconductor Tc Study Notes

Starting the analysis of superconducting materials to identify critical temperatures.

## Step A: Series Peak
Need to measure the Tc values for the series members in measurable.txt to find the composition with maximum Tc.

The seed composition is O6.080Ca1Cu2Ba2Hg1.

Measuring series members that belong to the same homologous doping series (same element set, varying oxygen content).

Measurement results:
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K
- O6.220Ca1Cu2Ba2Hg1: 127.0K
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K
- O4.096Cu1Ba2Hg1: 95.6K
- O4.097Cu1Ba2Hg1: 95.3K
- O4.047Cu1Ba2Hg1: 95.0K

Maximum Tc (T*) is 127.0K for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1.

## Step B: Filter family B
Family B contains materials that contain Hg and have exactly 4 elements.
Threshold1 = T* - 32 = 127.0 - 32 = 95.0K

From the measurable.txt file, I identified these materials that contain Hg and have exactly 4 elements:
- O4.081Cu1Ba2Hg1: 98.0K >= 95.0K ✓
- O4.162Cu1Ba2Hg1: 97.4K >= 95.0K ✓
- O4.150Cu1Ba2Hg1: 97.0K >= 95.0K ✓
- O4.220Cu1Ba2Hg1: 97.0K >= 95.0K ✓
- O4.096Cu1Ba2Hg1: 95.6K >= 95.0K ✓
- O4.097Cu1Ba2Hg1: 95.3K >= 95.0K ✓
- O4.047Cu1Ba2Hg1: 95.0K >= 95.0K ✓

However, I notice I haven't measured these compositions from the measurable.txt file yet:
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Unfortunately, I've used all my measurement budget of 18 calls. I need to work with the measurements I have.

Based on the measurements I took:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1) with Tc = 127.0K
- Threshold = 127.0 - 32 = 95.0K
- Materials containing Hg with exactly 4 elements that meet the threshold: All the O-Cu-Ba-Hg materials I measured have Tc >= 95.0K

For Step C: I need to analyze the patterns to predict for unmeasurable materials.
Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 (similar to the high Tc Ca-Cu-Ba-Hg series, likely high)
- O6.270Ca1Cu2Ba2Hg1 (similar to the high Tc Ca-Cu-Ba-Hg series, likely high)
- O4.180Cu1Ba2Hg1 (similar to the 4-element O-Cu-Ba-Hg series, likely high based on pattern)
- O8Ca0.850Cu2.200Ba2Tl1.800 (complex with 5 elements, possibly low)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (different structure with Sr instead of Ba, possibly low)
- O8Cu3Y1Ba1.900La0.100 (different structure with Y instead of Ca, possibly low)
- O8Cu4Y1Ba1.870La0.130 (different structure with Y instead of Ca, possibly low)

Looking at the pattern, materials with the Ca-Cu-Ba-Hg structure tend to have higher Tc values, especially around certain oxygen doping levels. Materials with substitutions like Y/La instead of Ca, or Sr instead of Ba, or additional elements might have lower Tc values.

Rule: Materials with the basic structure of Ca-Cu-Ba-Hg with appropriate oxygen doping (around 6.2 oxygen atoms) tend to have high Tc values (>90K), while materials with significant structural changes (different cations replacing key elements) or many additional elements tend to have lower Tc values.

Predictions:
- O6.160Ca1Cu2Ba2Hg1: high (similar structure to high Tc materials)
- O6.270Ca1Cu2Ba2Hg1: high (similar structure to high Tc materials)
- O4.180Cu1Ba2Hg1: high (follows the 4-element O-Cu-Ba-Hg pattern)
- O8Ca0.850Cu2.200Ba2Tl1.800: low (too many different elements, complex structure)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (Sr instead of Ba, additional elements)
- O8Cu3Y1Ba1.900La0.100: low (Y instead of Ca, different structure)
- O8Cu4Y1Ba1.870La0.130: low (Y instead of Ca, different structure)

## Step D: Final pick
Among unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1
Number of elements in each:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)

O4.180Cu1Ba2Hg1 has the fewest elements (4), so this is the final pick.