# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 34 calls to measure_tc

## Step A: Series Peak
Results of measuring series members in data/measurable.txt:

Ca1Cu2Ba2Hg1 series (varying oxygen):
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

O-Cu-Ba-Hg series (varying oxygen):
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

Other materials:
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

Maximum Tc (T*) = 127.0 K from compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)

## Step B: Filter Family B
Materials containing Hg with exactly 4 elements, with Tc >= T* - 32
Threshold = 127.0 - 32 = 95.0 K

Materials containing Hg with exactly 4 elements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (3 elements: Ca, Cu, Ba, Hg - wait, actually has 5 elements: O, Ca, Cu, Ba, Hg)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K 
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K (4 elements: O, Cu, Ba, Hg)
- O4.162Cu1Ba2Hg1: 97.4 K (4 elements: O, Cu, Ba, Hg)
- O4.150Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg)
- O4.220Cu1Ba2Hg1: 97.0 K (4 elements: O, Cu, Ba, Hg)
- O4.096Cu1Ba2Hg1: 95.6 K (4 elements: O, Cu, Ba, Hg)
- O4.097Cu1Ba2Hg1: 95.3 K (4 elements: O, Cu, Ba, Hg)
- O4.047Cu1Ba2Hg1: 95.0 K (4 elements: O, Cu, Ba, Hg)
- O4.200Cu1Ba2Hg1: 94.2 K (4 elements: O, Cu, Ba, Hg) - below threshold
- O4.101Cu1Ba2Hg1: 93.4 K (4 elements: O, Cu, Ba, Hg) - below threshold
- O4.103Cu1Ba2Hg1: 92.5 K (4 elements: O, Cu, Ba, Hg) - below threshold
- O4.085Cu1Ba2Hg1: 92.0 K (4 elements: O, Cu, Ba, Hg) - below threshold

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

Wait, let me reconsider the element counting. Looking at O4.081Cu1Ba2Hg1:
- Elements: O, Cu, Ba, Hg = 4 elements ✓
Looking at O6.080Ca1Cu2Ba2Hg1:
- Elements: O, Ca, Cu, Ba, Hg = 5 elements

Corrected Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C: Extrapolate
Predict high/low for unmeasurable materials based on measurable ones

Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg) - similar to high Tc series with Hg
- O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg) - similar to high Tc series with Hg
- O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - contains Hg, 4 elements like high Tc series
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl) - no Hg, mixed elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb) - complex mix
- O8Cu3Y1Ba1.900La0.100 (5 elements: O, Cu, Y, Ba, La) - no Hg, mixed rare earth
- O8Cu4Y1Ba1.870La0.130 (5 elements: O, Cu, Y, Ba, La) - no Hg, mixed rare earth

Analyzing patterns from measurable materials:
High Tc materials (>= 90K):
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.047Cu1Ba2Hg1: 95.0 K

Low Tc materials (< 90K):
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K (surprisingly high despite having 5 elements)

Based on the data, I notice:
1. Materials containing Hg with simple compositions (4 elements) tend to have high Tc
2. Materials with too many different element types (like Tl, Cd, La mixed together) tend to have lower Tc
3. The oxygen stoichiometry is important - in the Ca1Cu2Ba2Hg1 series, oxygen values around 6.19-6.24 show highest Tc
4. The presence of Hg seems beneficial when combined with Cu, Ba and either Ca or O

Rule: Materials with Hg and fewer different element types (4 or fewer) tend to have high Tc. Complex mixtures of different metals tend to have lower Tc.

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: HIGH (similar to high Tc series, contains Hg)
- O6.270Ca1Cu2Ba2Hg1: LOW (oxygen value likely too high compared to peak in series)
- O4.180Cu1Ba2Hg1: HIGH (4 elements, contains Hg, similar to high Tc series)
- O8Ca0.850Cu2.200Ba2Tl1.800: LOW (no Hg, mixed elements)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (too many different element types)
- O8Cu3Y1Ba1.900La0.100: LOW (no Hg, mixed rare earth elements)
- O8Cu4Y1Ba1.870La0.130: LOW (no Hg, mixed rare earth elements)

## Step D: Final Pick
From the unmeasurable materials predicted as high:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)

Since O4.180Cu1Ba2Hg1 has fewer elements (4) than O6.160Ca1Cu2Ba2Hg1 (5), the final pick is O4.180Cu1Ba2Hg1.