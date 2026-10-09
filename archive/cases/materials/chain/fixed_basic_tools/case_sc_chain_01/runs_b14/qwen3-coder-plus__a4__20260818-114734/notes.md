# Superconductor Tc Study Notes

## Step A - Series Peak
Measuring the seed material and other series members to find the composition with maximum Tc.

Results from measurements:

Series 1: O6.xCa1Cu2Ba2Hg1
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

Series 2: O4.xCu1Ba2Hg1
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.220Cu1Ba2Hg1: 97.0 K

Maximum Tc found: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

So T* = 127.0 K

## Step B - Filter family B
Family B: Materials containing Hg with exactly 4 elements
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Let me identify which measurable materials contain Hg and have exactly 4 elements:
- O6.080Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.100Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.130Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.140Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.190Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.210Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.220Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.240Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.300Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O6.310Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O) - No
- O4.081Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes
- O4.162Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes
- O4.150Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.220Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes
- O4.096Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.097Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.047Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.200Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.101Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.103Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)
- O4.085Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O) - Yes (Tc not measured)

Of the ones I did measure:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓

I didn't get to measure all of the Hg-containing 4-element materials due to budget limitations. I can only include those I actually measured.

Wait, let me reconsider the definition of "elements". In materials science, we often distinguish between cationic elements and oxygen separately. Let me re-evaluate:

- O6.080Ca1Cu2Ba2Hg1: Elements are Ca, Cu, Ba, Hg (4 cationic elements), O (oxygen) - depends on definition
- O4.081Cu1Ba2Hg1: Elements are Cu, Ba, Hg (3 cationic elements), O (oxygen) - 4 total if counting O

Based on the problem statement, let me interpret "exactly 4 elements" as meaning exactly 4 distinct atomic species, so including oxygen:
- O4.081Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.162Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.220Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.150Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.096Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.097Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.047Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.200Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.101Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.103Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓
- O4.085Cu1Ba2Hg1: O, Cu, Ba, Hg = 4 elements ✓

For materials with 5 components like O6.080Ca1Cu2Ba2Hg1: O, Ca, Cu, Ba, Hg = 5 elements (not in family B)

So Q1 includes the materials I measured that contain Hg and have exactly 4 elements with Tc >= 95.0 K:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓

## Step C - Extrapolate
For materials in unmeasurable.txt, I need to induce a rule that separates high (>90K) from low Tc materials based on the measurable ones I tested.

Materials I could have measured from the measurable list but didn't due to budget:
- O4.150Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Looking at the measurable data I collected:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high - 5 elements)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high - 5 elements)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high - 5 elements)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high - 5 elements)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high - 5 elements)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high - 5 elements)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high - 5 elements)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high - 5 elements)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high - 5 elements)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high - 5 elements)
- O4.081Cu1Ba2Hg1: 98.0 K (high - 4 elements)
- O4.162Cu1Ba2Hg1: 97.4 K (high - 4 elements)
- O4.220Cu1Ba2Hg1: 97.0 K (high - 4 elements)

All measured materials with Hg are high Tc (>90K). 

Let me look for a pattern that distinguishes high vs low Tc materials. Based on the limited data I have:

Rule: All materials containing Hg seem to be high Tc, but I need to check if this is sufficient. Looking at the measurable list, I notice that all materials I measured had high Tc values, so I need to consider the materials I didn't measure.

Let me look at the full measurable list again to understand what might be low Tc:
- O6.080Ca1Cu2Ba2Hg1: high (measured)
- O6.100Ca1Cu2Ba2Hg1: high (measured)
- ...
- O6Cu1Ba2Hg1Tl1: has Hg (5 elements)
- O10Ca2Cu3Ba2Tl2: no Hg (5 elements)
- O6Cu1Cd0.250Ba2Tl1.750: no Hg (5 elements)
- O7Cu3Ba2La1: no Hg (4 elements)
- O7Cu3Y0.200Ba2La0.800: no Hg (5 elements)

Looking for a pattern in my data, I see that all materials containing Hg were high Tc. But I need to consider that I didn't measure all materials.

From the little data I have, it appears that materials containing Hg tend to have high Tc values. For materials without Hg, it seems to depend on other factors.

Based on my analysis, I'll propose the following rule: Materials with Hg are likely high Tc, while materials without Hg but with certain combinations of other elements (like rare earth elements) can also be high Tc.

Actually, let me reconsider more carefully. Since all the materials I measured turned out to be high Tc, I need to make predictions based on the pattern I can observe from the oxygen content or other features in the unmeasured materials.

Looking more systematically:
- Materials with Hg and 4 elements: all high Tc so far
- Materials with Hg and 5 elements: all high Tc so far

For materials without Hg, I need to look for patterns. But I didn't measure any of the non-Hg materials.

Let me think differently. Maybe I can predict based on similarity to the series I did measure:
- O6.160Ca1Cu2Ba2Hg1: Similar to O6.xCa1Cu2Ba2Hg1 series, probably high
- O6.270Ca1Cu2Ba2Hg1: Similar to O6.xCa1Cu2Ba2Hg1 series, probably high (between O6.240 and O6.300)
- O4.180Cu1Ba2Hg1: Similar to O4.xCu1Ba2Hg1 series, probably high (between O4.162 and O4.220)
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl, no Hg, 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Sr, Pb, Tl, no Hg, 5 elements
- O8Cu3Y1Ba1.900La0.100: Contains Y, La, no Hg, 4 elements
- O8Cu4Y1Ba1.870La0.130: Contains Y, La, no Hg, 4 elements

Based on the series trend I observed (O6.xCa1Cu2Ba2Hg1 showed a peak around O6.210-O6.220), I predict:
- O6.160Ca1Cu2Ba2Hg1: high Tc (likely > 105K based on trend)
- O6.270Ca1Cu2Ba2Hg1: high Tc (likely > 120K based on trend)
- O4.180Cu1Ba2Hg1: high Tc (~97-98K based on trend)

For the others without Hg, they may have lower Tc, but I need to be more careful since I don't have direct evidence.

Looking more carefully, materials with rare earth elements like Y, La might also have high Tc. So my rule might be:
Materials with Hg OR with rare earth elements (Y, La, etc.) AND appropriate structure are high Tc.

Predictions:
- O6.160Ca1Cu2Ba2Hg1: high (contains Hg)
- O6.270Ca1Cu2Ba2Hg1: high (contains Hg)
- O4.180Cu1Ba2Hg1: high (contains Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800: high (contains Tl which is similar to Hg group)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (no Hg, no rare earth, complex structure might reduce Tc)
- O8Cu3Y1Ba1.900La0.100: high (contains Y and La - rare earth elements)
- O8Cu4Y1Ba1.870La0.130: high (contains Y and La - rare earth elements)

## Step D - Final Pick
Among unmeasurable materials predicted high, find the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O)
- O4.180Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (Ca, Cu, Ba, Tl, O)
- O8Cu3Y1Ba1.900La0.100: 5 elements (Cu, Y, Ba, La, O)
- O8Cu4Y1Ba1.870La0.130: 5 elements (Cu, Y, Ba, La, O)

The one with fewest elements among the high predictions is O4.180Cu1Ba2Hg1 with 4 elements.
