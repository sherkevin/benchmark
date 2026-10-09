# Notes for Superconductor Tc Study

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 18 calls to measure_tc
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
Results from measurements:

### Series 1: OxCa1Cu2Ba2Hg1 (varying oxygen)
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

Maximum Tc in this series: 127.0 K for O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0 K and the peak composition is either O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1

### Series 2: OxCu1Ba2Hg1 (varying oxygen)
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.047Cu1Ba2Hg1: 95.0 K
- O4.200Cu1Ba2Hg1: 94.2 K

Maximum Tc in this series: 98.0 K for O4.081Cu1Ba2Hg1

Overall maximum Tc: 127.0 K for O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1

Since we need a single peak composition, I'll select O6.210Ca1Cu2Ba2Hg1 as the peak composition.

Step A Result:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- T* (maximum Tc): 127.0 K

## Step B - Filter family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

From our measurements, I need to identify materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0 K

Looking at the measurable materials:
- O6.080Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.100Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.130Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.140Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.190Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.210Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.220Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.240Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.300Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify
- O6.310Ca1Cu2Ba2Hg1: Contains Hg, Elements = {O, Ca, Cu, Ba, Hg} = 5 elements → Doesn't qualify

For the OxCu1Ba2Hg1 series:
- O4.081Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 98.0 K ≥ 95.0 K → Qualifies
- O4.162Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 97.4 K ≥ 95.0 K → Qualifies
- O4.150Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 97.0 K ≥ 95.0 K → Qualifies
- O4.220Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 97.0 K ≥ 95.0 K → Qualifies
- O4.096Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 95.6 K ≥ 95.0 K → Qualifies
- O4.097Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 95.3 K ≥ 95.0 K → Qualifies
- O4.047Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 95.0 K ≥ 95.0 K → Qualifies
- O4.200Cu1Ba2Hg1: Contains Hg, Elements = {O, Cu, Ba, Hg} = 4 elements, Tc = 94.2 K < 95.0 K → Doesn't qualify

Wait, let me reconsider the elements counting. Looking at O6.080Ca1Cu2Ba2Hg1:
Elements: O, Ca, Cu, Ba, Hg = 5 elements (not 4)
So none of the Ca-containing compounds will qualify as they all have 5 elements.

The qualifying materials (Q1) are:
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

Step B Results:
- Threshold1 = 95.0 K
- Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]

## Step C - Extrapolate for unmeasurable materials

Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Based on my analysis of the measurable materials, I can induce the following rule to separate high from low Tc:

Rule: Materials with fewer elements (4 elements vs 5 elements) tend to have lower Tc when they contain similar components. The presence of Ca seems to boost Tc significantly compared to similar compounds without Ca. Also, oxygen content plays a role - there appears to be an optimal oxygen content around 6.21-6.22 for the Ca-containing compounds and around 4.08 for the simpler compounds.

Looking at the patterns:
- Ca-containing compounds (5 elements): Higher Tc range (92-127 K), with peak around O6.21-O6.22
- Simple compounds (4 elements): Lower Tc range (94-98 K)

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: Similar to high-Tc series, likely high (~125 K based on trend)
- O6.270Ca1Cu2Ba2Hg1: Similar to high-Tc series but beyond peak, likely high (~120 K based on trend)
- O4.180Cu1Ba2Hg1: Similar to 4-element series, likely low (~95 K based on trend)
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ca and additional Tl, complex, likely high
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ca but replaces Ba with Sr and adds Pb/Tl, likely high
- O8Cu3Y1Ba1.900La0.100: No Ca but Y/Ba/La instead, likely low
- O8Cu4Y1Ba1.870La0.130: No Ca but Y/Ba/La instead, likely low

## Step D - Final pick
Among the unmeasurable materials predicted high in step C, I need to return the one with the fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 6 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Both O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 have 5 elements. Between them, O6.160Ca1Cu2Ba2Hg1 is closer to the peak oxygen content of 6.21-6.22, so it might have higher Tc.

Final pick: O6.160Ca1Cu2Ba2Hg1