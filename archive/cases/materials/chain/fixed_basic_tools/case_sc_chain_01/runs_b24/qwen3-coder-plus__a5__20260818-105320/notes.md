# Notes for Superconductor Tc Study

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 24 measurements
- High Tc: >= 90 K, Low Tc: < 90 K

## Step A - Series Peak
Measured compositions from data/measurable.txt:

### Series 1: O6.xCa1Cu2Ba2Hg1
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

MAXIMUM Tc = 127.0 K for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0 K

### Series 2: O4.xCu1Ba2Hg1
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

### Others:
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K

Step A Result:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)
- T* = 127.0 K

## Step B - Filter family B
Family B: Contains Hg and has exactly 4 elements
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg with exactly 4 elements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (not >= 95.0)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (not >= 95.0)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (>= 95.0) ✓
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (>= 95.0) ✓
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (>= 95.0) ✓
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (>= 95.0) ✓
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (>= 95.0) ✓
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (>= 95.0) ✓
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (>= 95.0) ✓
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (>= 95.0) ✓
- O4.081Cu1Ba2Hg1: 98.0 K (>= 95.0) ✓
- O4.162Cu1Ba2Hg1: 97.4 K (>= 95.0) ✓
- O4.150Cu1Ba2Hg1: 97.0 K (>= 95.0) ✓
- O4.220Cu1Ba2Hg1: 97.0 K (>= 95.0) ✓
- O4.096Cu1Ba2Hg1: 95.6 K (>= 95.0) ✓
- O4.097Cu1Ba2Hg1: 95.3 K (>= 95.0) ✓
- O4.047Cu1Ba2Hg1: 95.0 K (>= 95.0) ✓
- O4.200Cu1Ba2Hg1: 94.2 K (< 95.0)
- O4.101Cu1Ba2Hg1: 93.4 K (< 95.0)
- O4.103Cu1Ba2Hg1: 92.5 K (< 95.0)
- O4.085Cu1Ba2Hg1: 92.0 K (< 95.0)

Q1 = [O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1, O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]

## Step C - Extrapolate rule
Materials in data/unmeasurable.txt:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Let me analyze patterns in the measurable data to form a rule:

High Tc materials (>=90K):
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
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc materials (<90K):
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K

Analyzing elements:
Looking at the compositions, I notice:
1. The presence of Hg alone isn't sufficient (O6Cu1Ba2Hg1Tl1 has 0.0 K)
2. Having exactly 4 elements with Hg seems important for high Tc
3. Oxygen content appears to affect Tc in the Ca-Cu-Ba-Hg system
4. In the O6.xCa1Cu2Ba2Hg1 series, Tc peaks around O6.21-O6.22
5. In the O4.xCu1Ba2Hg1 series, all are high Tc with values between 92-98 K
6. The compound O6Cu1Cd0.250Ba2Tl1.750 (88K) has 5 elements and is low Tc
7. The compound O6Cu1Ba2Hg1Tl1 (0K) has 5 elements and is very low Tc

The rule appears to be: A material is likely high Tc if it contains Hg and has exactly 4 elements. The oxygen content affects the actual Tc value within that group.

Let me verify this rule with the measurable data:
- O6Cu1Ba2Hg1Tl1: 5 elements, contains Hg, 0.0 K (low Tc) - fits rule
- O6Cu1Cd0.250Ba2Tl1.750: 5 elements, no Hg, 88.0 K (low Tc) - fits rule
- O10Ca2Cu3Ba2Tl2: 5 elements, no Hg, 118.97 K (high Tc) - exception to simple rule!

Wait, O10Ca2Cu3Ba2Tl2 doesn't contain Hg but still has high Tc. This complicates things.
Let me reconsider...

Actually, looking more carefully:
- O10Ca2Cu3Ba2Tl2: Contains Tl, not Hg, but has 5 elements and high Tc
- So the rule isn't just "Hg and 4 elements"

Let me think differently:
- High Tc materials tend to have Cu and Ba
- Hg seems to boost Tc significantly in 4-element systems
- Tl also appears to contribute to high Tc (like in O10Ca2Cu3Ba2Tl2)
- The combination of Cu+Ba with either Hg or Tl seems important
- The number of elements might matter too - 4-element systems with Hg show high Tc

Actually, let me reconsider the rule based on the measurable data again:
Looking at 4-element systems with Hg vs others:
- 4-element systems with Hg: mostly high Tc (except maybe at extreme oxygen values)
- 5-element systems: mixed results

Rule hypothesis: Materials with Hg AND exactly 4 elements are likely high Tc, but there might be other combinations too.

Actually, looking at the definition again: "filter family B (contains Hg and has exactly 4 elements)" - this suggests that we're focusing on this specific family for step B, which supports the idea that Hg + 4 elements is a significant factor.

For the unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 4 elements, has Hg → likely high (interpolated between O6.140 and O6.190)
- O6.270Ca1Cu2Ba2Hg1: 4 elements, has Hg → likely high (interpolated between O6.240 and O6.300)
- O4.180Cu1Ba2Hg1: 4 elements, has Hg → likely high (interpolated between O4.162 and O4.220)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements, has Tl but no Hg → prediction depends on whether Tl follows similar rules to Hg
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 5 elements, has Tl and Pb but no Hg → likely low
- O8Cu3Y1Ba1.900La0.100: 5 elements, no Hg, has Y and La → likely low
- O8Cu4Y1Ba1.870La0.130: 5 elements, no Hg, has Y and La → likely low

Wait, let me look at O8Ca0.850Cu2.200Ba2Tl1.800 more carefully: elements are Ca, Cu, Ba, Tl (4 different elements? No, counting Tl separately: Ca, Cu, Ba, Tl = 4 elements). Yes, this has 4 elements and Tl (not Hg).

Looking back at O10Ca2Cu3Ba2Tl2: elements are Ca, Cu, Ba, Tl = 4 elements, no Hg, high Tc (118.97 K)

So the rule might be: 4-element systems with either Hg or Tl and containing Cu+Ba tend to be high Tc.

Refined rule: A material is high Tc if it has exactly 4 elements AND contains Cu+Ba AND contains either Hg or Tl.

Let me verify with known data:
- O6.080Ca1Cu2Ba2Hg1: 4 elem, Cu+Ba, Hg → 92.0 K (high) ✓
- O6.100Ca1Cu2Ba2Hg1: 4 elem, Cu+Ba, Hg → 92.5 K (high) ✓
- ...
- O6Cu1Ba2Hg1Tl1: 5 elem, Cu+Ba, Hg+Tl → 0.0 K (low) - breaks rule due to 5 elements
- O10Ca2Cu3Ba2Tl2: 4 elem, Cu+Ba, Tl → 118.97 K (high) ✓
- O6Cu1Cd0.250Ba2Tl1.750: 5 elem, Cu+Ba, Cd+Tl → 88.0 K (low) - breaks rule due to 5 elements

Perfect! This refined rule works well: 4 elements + Cu + Ba + (Hg OR Tl) = high Tc

Applying to unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 4 elem, Cu+Ba, Hg → high
- O6.270Ca1Cu2Ba2Hg1: 4 elem, Cu+Ba, Hg → high  
- O4.180Cu1Ba2Hg1: 4 elem, Cu+Ba, Hg → high
- O8Ca0.850Cu2.200Ba2Tl1.800: 4 elem, Cu+Ba, Tl → high
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 5 elem, Cu+Sr, Tl+Pb → low (doesn't meet criteria)
- O8Cu3Y1Ba1.900La0.100: 5 elem, Cu+Ba, Y+La → low (doesn't meet criteria)
- O8Cu4Y1Ba1.870La0.130: 5 elem, Cu+Ba, Y+La → low (doesn't meet criteria)

## Step D - Final Pick
Unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800

Find the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 4 elements
- O6.270Ca1Cu2Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 4 elements

All have 4 elements, so break ties by higher expected Tc.
Based on the trends from measurable data:
- The Hg-containing ones likely have higher Tc than Tl-containing ones
- For the Ca-Cu-Ba-Hg series, peak is around O6.21-O6.22, so O6.270 should have high Tc
- O6.160 would be lower than O6.270 based on trend
- O4.180Cu1Ba2Hg1 should have moderate Tc (~95-98K based on series)
- O8Ca0.850Cu2.200Ba2Tl1.800 would have high Tc but probably not as high as optimal Hg compounds

So the ranking would be:
1. O6.270Ca1Cu2Ba2Hg1 (likely highest Tc, same system as peak)
2. O6.160Ca1Cu2Ba2Hg1 
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800

Final answer for Step D: O6.270Ca1Cu2Ba2Hg1