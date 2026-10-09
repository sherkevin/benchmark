# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 34 calls to measure_tc

## Step A: Series Peak
Need to measure the series members in data/measurable.txt and find the composition with MAXIMUM Tc.

Series members appear to be:
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

And also:
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1

Plus some others that may not be part of the same series...

## Step A Results
Measured all available compositions from measurable.txt:

O6 series (Ca, Cu, Ba, Hg):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: not available for measurement
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAX!)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (MAX!)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: not available for measurement
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

O4 series (Cu, Ba, Hg):
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.180Cu1Ba2Hg1: not available for measurement
- O4.047Cu1Ba2Hg1: not available for measurement
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

Other compositions:
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

Step A Result: MAX Tc = 127.0 K, corresponding to compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0 K

## Step B: Filter family B
Family B = materials that contain Hg and have exactly 4 elements
threshold1 = T* - 32 = 127.0 - 32 = 95.0

From the measurable compositions, I need to identify those that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0

Let me analyze the compositions containing Hg with exactly 4 elements:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 K < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 K < 95.0 ✗
- O4.085Cu1Ba2Hg1: 92.0 K < 95.0 ✗

Note: O6Cu1Ba2Hg1Tl1 contains 5 elements (O, Cu, Ba, Hg, Tl), so it doesn't qualify.

Therefore Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}

threshold1 = 95.0

## Step C: Rule Induction and Prediction for Unmeasurable Materials

First, let me list the unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 (same elements as seed series)
- O6.270Ca1Cu2Ba2Hg1 (same elements as seed series)
- O4.180Cu1Ba2Hg1 (same elements as O4 series)
- O4.047Cu1Ba2Hg1 (same elements as O4 series)
- O4.066Cu1Ba2Hg1 (same elements as O4 series)
- O4.160Cu1Ba2Hg1 (same elements as O4 series)
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Now I need to analyze the measurable materials to find patterns that separate high (>=90K) from low (<90K) Tc:

High Tc materials (>=90K):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (Actually LOW!)

Wait, O6Cu1Cd0.250Ba2Tl1.750 has Tc=88.0 which is < 90K, so it's low.

Low Tc materials (<90K):
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K

Looking at patterns:

Let me think about what makes a material high vs low Tc:

Pattern Analysis:
1. Elements present: Looking at the composition elements
2. Oxygen content: May play a role
3. Presence of certain elements like Tl, La, Cd might affect Tc negatively

High Tc examples:
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (Ca, Cu, Ba, Hg, O)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (Ca, Cu, Ba, Hg, O)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (Ca, Cu, Ba, Hg, O)
- O10Ca2Cu3Ba2Tl2: 118.97 K (Ca, Cu, Ba, Tl, O) - has Tl but still high
- O4.081Cu1Ba2Hg1: 98.0 K (Cu, Ba, Hg, O)

Low Tc examples:
- O6Cu1Ba2Hg1Tl1: 0.0 K (Cu, Ba, Hg, Tl, O) - has Tl and is very low
- O7Cu3Ba2La1: 89.6 K (Cu, Ba, La, O) - has La, low
- O7Cu3Y0.200Ba2La0.800: 81.0 K (Cu, Y, Ba, La, O) - has La, low
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (Cu, Cd, Ba, Tl, O) - has Cd and Tl, low

Wait, there's an interesting pattern:
- O10Ca2Cu3Ba2Tl2: 118.97 K (Ca, Cu, Ba, Tl, O) - has Tl but HIGH Tc
- O6Cu1Ba2Hg1Tl1: 0.0 K (Cu, Ba, Hg, Tl, O) - has Tl and LOW Tc

So it's not just having Tl, but the combination of elements. The presence of Hg seems to make Tl problematic.

Let me try to find a better rule:
- High Tc: Contains Ca, Cu, Ba, Hg with appropriate oxygen levels OR Cu, Ba, Hg, O with appropriate oxygen levels
- Low Tc: Contains La, Y, or combinations that interfere with superconductivity

Another pattern:
- When Hg is present WITHOUT Ca, Tc is generally lower
- When Ca, Cu, Ba, Hg are together with proper oxygen, Tc is high
- When La or Y are present instead of Ca, Tc tends to be lower

Let me analyze oxygen content more specifically for the series with similar elements:

For O4.xCu1Ba2Hg1 series (all have 4+X oxygen, 4 elements total):
- Higher oxygen values (around 4.08-4.22) tend to have higher Tc (92-98K)
- Lower oxygen values likely have lower Tc

For O6.xCa1Cu2Ba2Hg1 series:
- Oxygen around 6.14-6.24 gives high Tc (100-127K)
- Lower (6.08) and higher (6.30+) oxygen gives moderate-high Tc

Rule hypothesis: 
Materials are high Tc if they satisfy one of these conditions:
1. They have the base formula of either O6.xCa1Cu2Ba2Hg1 with oxygen content between ~6.10 and ~6.24, OR
2. They have the base formula of O4.xCu1Ba2Hg1 with oxygen content between ~4.08 and ~4.22 (but excluding very low oxygen values)
3. They have certain element combinations that promote superconductivity without problematic elements like La, Y

Actually, let me reconsider more systematically:

Looking at the specific compositions again:

High Tc (>90K):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (Ca-Cu-Ba-Hg-O system)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K (Cu-Ba-Hg-O system)
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K (Ca-Cu-Ba-Tl-O system)

Low Tc (<90K):
- O6Cu1Ba2Hg1Tl1: 0.0 K (Cu-Ba-Hg-Tl-O system) - note: has Hg+Tl, no Ca
- O7Cu3Ba2La1: 89.6 K (Cu-Ba-La-O system) - note: has La instead of Ca/Ba
- O7Cu3Y0.200Ba2La0.800: 81.0 K (Cu-Y-Ba-La-O system) - note: has La, Y
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (Cu-Cd-Ba-Tl-O system) - note: has Cd, Tl

Key insight: The presence of La or Y appears to correlate with low Tc. Also, Hg+Tl without Ca seems problematic.

Rule: A material is high Tc if it:
1. Contains the core superconducting elements (typically Cu + Ba + O) with either:
   a) Ca and Hg (with appropriate oxygen doping)
   b) Just Hg (with appropriate oxygen doping)  
   c) Ca and Tl (like O10Ca2Cu3Ba2Tl2)
AND
2. Does NOT contain La or Y
AND
3. Has appropriate oxygen stoichiometry

Now predicting for unmeasurable materials:

- O6.160Ca1Cu2Ba2Hg1: Same system as high Tc series, has Ca+Cu+Ba+Hg, no La/Y → HIGH
- O6.270Ca1Cu2Ba2Hg1: Same system as high Tc series, has Ca+Cu+Ba+Hg, no La/Y → HIGH (though at the edge of optimal range)
- O4.180Cu1Ba2Hg1: Same system as high Tc O4 series, has Cu+Ba+Hg, no La/Y → HIGH
- O4.047Cu1Ba2Hg1: Same system as high Tc O4 series, has Cu+Ba+Hg, no La/Y → HIGH (though at low oxygen end)
- O4.066Cu1Ba2Hg1: Same system as high Tc O4 series, has Cu+Ba+Hg, no La/Y → HIGH (though at low oxygen end)
- O4.160Cu1Ba2Hg1: Same system as high Tc O4 series, has Cu+Ba+Hg, no La/Y → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Has Ca+Cu+Ba+Tl, no La/Y → HIGH (similar to O10Ca2Cu3Ba2Tl2)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Ca+Cu+Sr+Tl+Pb, no La/Y → HIGH (has Ca which is good, Sr instead of Ba but no La/Y)
- O8Cu3Y1Ba1.900La0.100: Has Y and La → LOW
- O8Cu4Y1Ba1.870La0.130: Has Y and La → LOW

## Step D: Final Pick

From Step C, the unmeasurable materials predicted as HIGH are:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg) 
3. O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
4. O4.047Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
5. O4.066Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
6. O4.160Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
7. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

Among these, the ones with the fewest elements (4 elements) are:
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements

To break the tie, I need to consider expected Tc. Based on the pattern from measured O4.xCu1Ba2Hg1 materials:
- Higher oxygen content around 4.15-4.22 had higher Tc (97-98K)
- Lower oxygen content around 4.05-4.10 had lower Tc (92-96K)

Ranked by expected Tc (highest first):
- O4.180Cu1Ba2Hg1: Expected ~97-98K (similar to O4.162 which was 97.4K and O4.220 which was 97.0K)
- O4.160Cu1Ba2Hg1: Expected ~97.4K (interpolated from neighbors)
- O4.066Cu1Ba2Hg1: Expected ~93-95K (similar to O4.081 which was 98.0K but this is slightly lower oxygen)
- O4.047Cu1Ba2Hg1: Expected ~92-94K (similar to O4.085 which was 92.0K but this is lower oxygen)

Therefore, the final pick is: O4.180Cu1Ba2Hg1 (4 elements, expected highest Tc among 4-element materials)