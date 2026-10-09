# Superconductor Tc Study Notes

## Step A - Series Peak
Measuring the series members in data/measurable.txt to find the composition with MAXIMUM Tc.

Starting with the seed material: O6.080Ca1Cu2Ba2Hg1

I will measure all compositions in the series that vary oxygen:
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

RESULT: Peak Tc is 127.0K for both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
T* = 127.0K

## Step B - Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Materials containing Hg and having exactly 4 elements:
- O4.081Cu1Ba2Hg1: 98.0K >= 95.0K ✓
- O4.162Cu1Ba2Hg1: 97.4K >= 95.0K ✓
- O4.150Cu1Ba2Hg1: 97.0K >= 95.0K ✓
- O4.220Cu1Ba2Hg1: 97.0K >= 95.0K ✓
- O4.096Cu1Ba2Hg1: 95.6K >= 95.0K ✓
- O4.097Cu1Ba2Hg1: 95.3K >= 95.0K ✓
- O4.047Cu1Ba2Hg1: 95.0K >= 95.0K ✓
- O4.200Cu1Ba2Hg1: 94.2K < 95.0K ✗

Q1 = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.047Cu1Ba2Hg1"]

## Step C - Extrapolate (no measurement possible)
The materials in data/unmeasurable.txt have never been synthesized — there is no physical sample to put in the instrument.

Unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.180Cu1Ba2Hg1
- O8Ca0.850Cu2.200Ba2Tl1.800
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Cu3Y1Ba1.900La0.100
- O8Cu4Y1Ba1.870La0.130

Based on measurable data, I need to find a rule that separates high (>=90K) from low (<90K) Tc materials.

Looking at the measurable materials I tested:
High Tc (>=90K):
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
- O4.200Cu1Ba2Hg1: 94.2K

All the materials I tested were high Tc. I need to consider other measurable materials that weren't in the main series:

Wait, I see from measurable.txt there were more materials I didn't test because they didn't fit the criteria for Steps A & B:

- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Since I've used all my measurement budget (18/18), I cannot measure these additional materials. So based on the data I have:

Rule for predicting high vs low Tc:
Looking at the materials I could measure, most appear to be high Tc superconductors. The ones with highest Tc seem to be the O6.x Ca1Cu2Ba2Hg1 series with optimal oxygen doping around 6.21-6.22. 

For the unmeasurable materials, I need to make predictions based on structural similarity to known high Tc materials:

- O6.160Ca1Cu2Ba2Hg1: Similar to the high Tc Ca-Cu-Ba-Hg series → HIGH
- O6.270Ca1Cu2Ba2Hg1: Similar to the high Tc Ca-Cu-Ba-Hg series → HIGH  
- O4.180Cu1Ba2Hg1: Similar to the high Tc Cu-Ba-Hg series → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl instead of Hg, but similar structure → HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Different elements (Sr, Pb, Tl) → LOW
- O8Cu3Y1Ba1.900La0.100: Different structure with Y/Ba/La → Could be HIGH
- O8Cu4Y1Ba1.870La0.130: Different structure with Y/Ba/La → Could be HIGH

Actually, let me reconsider the rule. Looking again at the materials I measured:
- All materials I measured had Tc >= 92.0K, so they're all high Tc by definition
- The Hg-containing materials all showed high Tc
- The Ca-Cu-Ba-Hg-O series showed very high Tc (up to 127K)

So a good rule might be: Materials with Hg and proper oxygen doping show high Tc. Materials with rare earth elements like Y and La might also show high Tc. Complex multi-element materials with Tl/Pb/Sr might have lower Tc.

Revised predictions:
- O6.160Ca1Cu2Ba2Hg1: Contains Hg, similar to high Tc series → HIGH
- O6.270Ca1Cu2Ba2Hg1: Contains Hg, similar to high Tc series → HIGH
- O4.180Cu1Ba2Hg1: Contains Hg, similar to high Tc series → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl instead of Hg, but similar structure → MIGHT BE HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Multiple complex elements, less likely → LOW
- O8Cu3Y1Ba1.900La0.100: Contains Y and La which often form high Tc materials → HIGH
- O8Cu4Y1Ba1.870La0.130: Contains Y and La which often form high Tc materials → HIGH

Actually, since all measured materials were high Tc, I should look for distinguishing features:

Looking more carefully at the structures:
- Materials with Hg: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, etc. and O4.081Cu1Ba2Hg1, etc. -> all high Tc
- The oxygen content seems important: optimal appears to be around O6.2 for the Ca series and O4.0-4.2 for the simpler Cu-Ba-Hg series

Final predictions:
- O6.160Ca1Cu2Ba2Hg1: Contains Hg, oxygen content near optimal range → HIGH
- O6.270Ca1Cu2Ba2Hg1: Contains Hg, oxygen content near optimal range → HIGH
- O4.180Cu1Ba2Hg1: Contains Hg, oxygen content in measured range → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl (similar to Hg group), complex doping → MIGHT BE HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Complex mix of Sr, Tl, Pb, not in my measured families → LOW
- O8Cu3Y1Ba1.900La0.100: Contains Y/Ba/La, cuprate structure → HIGH
- O8Cu4Y1Ba1.870La0.130: Contains Y/Ba/La, cuprate structure → HIGH

## Step D - Final pick
Among unmeasurable materials predicted high, find the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← Fewest
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
- O8Cu3Y1Ba1.900La0.100: 5 elements (O, Cu, Y, Ba, La)
- O8Cu4Y1Ba1.870La0.130: 5 elements (O, Cu, Y, Ba, La)

The one with fewest elements is O4.180Cu1Ba2Hg1 with 4 elements.

However, I realize I should try to predict which would have the highest Tc among the 4-element ones if there are ties. Since O4.180Cu1Ba2Hg1 is in the same family as the measured O4.xCu1Ba2Hg1 series which showed high Tc (94.2K-98.0K), this seems like the right choice.