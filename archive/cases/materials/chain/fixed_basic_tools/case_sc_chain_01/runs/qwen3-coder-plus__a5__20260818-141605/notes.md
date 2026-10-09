# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 18 calls
- Definition: High Tc >= 90K, Low Tc < 90K

## Step A - Series Peak
Measured compositions from measurable.txt:

Series 1 (O6.xCa1Cu2Ba2Hg1):
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

Series 2 (O4.xCu1Ba2Hg1):
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K
- O4.096Cu1Ba2Hg1: 95.6K
- O4.097Cu1Ba2Hg1: 95.3K
- O4.047Cu1Ba2Hg1: 95.0K
- O4.200Cu1Ba2Hg1: 94.2K

Other measured compositions:
- O4.101Cu1Ba2Hg1: [Not measured - out of budget]
- O4.103Cu1Ba2Hg1: [Not measured - out of budget]
- O4.085Cu1Ba2Hg1: [Not measured - out of budget]
- O6Cu1Ba2Hg1Tl1: [Not measured - out of budget]
- O10Ca2Cu3Ba2Tl2: [Not measured - out of budget]
- O6Cu1Cd0.250Ba2Tl1.750: [Not measured - out of budget]
- O7Cu3Ba2La1: [Not measured - out of budget]
- O7Cu3Y0.200Ba2La0.800: [Not measured - out of budget]

Peak Tc found: 127.0K for O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0K and the peak composition is O6.210Ca1Cu2Ba2Hg1 (choosing the first one)

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0K

Materials containing Hg with exactly 4 elements from my measurements:
- O4.081Cu1Ba2Hg1: 98.0K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0K (4 elements: Cu, Ba, Hg, O) - meets criteria, Tc >= 95.0 ✓

Based on the pattern in the O4.xCu1Ba2Hg1 series, we can estimate:
- O4.101Cu1Ba2Hg1 would likely have Tc around 95-96K (interpolated between O4.096 and O4.103)
- O4.103Cu1Ba2Hg1 would likely have Tc around 95-96K (similar to O4.101)
- O4.085Cu1Ba2Hg1 would likely have Tc around 97-98K (between O4.081 and O4.096)

All of these would likely meet the threshold, so Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1}

But since I didn't measure the last three, I should only include the ones I actually measured:
Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Pattern Recognition for Predictions

Looking at the measured data, I can observe:
1. Materials with 5 elements (like O6.xCa1Cu2Ba2Hg1 series) have higher Tc values
2. Materials with 4 elements (like O4.xCu1Ba2Hg1 series) have moderate Tc values
3. The oxygen content seems to affect Tc significantly
4. The presence of certain elements (Ca vs absence of Ca) might affect Tc

For unmeasurable materials, I need to consider:
- O6.160Ca1Cu2Ba2Hg1: Part of the same series as the peak material, likely high Tc
- O6.270Ca1Cu2Ba2Hg1: Part of the same series, likely high Tc (between O6.240 and O6.300)
- O4.180Cu1Ba2Hg1: Part of O4.xCu1Ba2Hg1 series, likely moderate Tc around 96-97K
- O8Ca0.850Cu2.200Ba2Tl1.800: Complex with 5+ elements including Tl, possibly high Tc
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Sr instead of Ba, complex mix, uncertain
- O8Cu3Y1Ba1.900La0.100: Contains Y and La, possibly high Tc
- O8Cu4Y1Ba1.870La0.130: Similar to above but with Cu4, possibly high Tc

Based on the pattern that materials with higher complexity (more elements/types) and optimal oxygen doping tend to have higher Tc:
- O6.160Ca1Cu2Ba2Hg1: high (part of high Tc series)
- O6.270Ca1Cu2Ba2Hg1: high (part of high Tc series)
- O4.180Cu1Ba2Hg1: low (below 90K threshold, based on trend in O4 series)
- O8Ca0.850Cu2.200Ba2Tl1.800: high (complex, multi-element)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: high (complex, multi-element)
- O8Cu3Y1Ba1.900La0.100: high (typical high Tc cuprate)
- O8Cu4Y1Ba1.870La0.130: high (typical high Tc cuprate)

## Rule Induced
The rule appears to be: Materials with optimal oxygen doping in known high-Tc families (cuprates with rare earths/Ba/Ca, especially with Hg or Tl) tend to have high Tc. The specific oxygen stoichiometry is crucial - for the Ca-Ba-Cu-Hg-O family, oxygen content around 6.2 appears optimal. For simpler Hg-Ba-Cu-O systems, the Tc is moderate but consistent. Complex multi-element systems with mixed cations (Tl, Pb) or rare earths (Y, La) often achieve high Tc.

## Step D - Final Pick
From unmeasurable materials predicted as high: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130

Element counts:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)
- O8Cu3Y1Ba1.900La0.100: 5 elements (O, Cu, Y, Ba, La)
- O8Cu4Y1Ba1.870La0.130: 5 elements (O, Cu, Y, Ba, La)

Lowest element count among high predictions: 5 elements (tied between several)
Among these with 5 elements, the Ca-Ba-Cu-Hg-O series typically has higher Tc based on my measurements.
Between O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800, the first two are part of the highest Tc series I measured.
Based on the series trend, O6.270 would have slightly lower Tc than O6.160 (as O6.240 had lower Tc than O6.220/O6.210).
So O6.160Ca1Cu2Ba2Hg1 would likely have the highest Tc among the 5-element options.