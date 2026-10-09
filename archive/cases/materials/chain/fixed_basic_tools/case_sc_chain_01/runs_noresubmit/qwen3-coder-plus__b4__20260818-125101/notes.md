# Notes for Superconductor Tc Study

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 18 `measure_tc` calls

## Step A: Series Peak
- Need to measure all compositions in `data/measurable.txt` that belong to the homologous doping series (same element set, varying oxygen)
- Find the composition with MAXIMUM Tc
- Call that maximum T*

## Step B: Filter Family B
- threshold1 = T* - 32
- Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= threshold1
- Call this set Q1

## Step C: Extrapolate
- Materials in `data/unmeasurable.txt` cannot be measured (no physical samples)
- Need to induce a rule that separates high from low Tc using measurable materials
- Predict high/low for every unmeasurable material
- Single-factor rules are wrong - need multi-factor interactions

## Step D: Final Pick
- Among unmeasurable materials predicted high in step C, return the one with fewest elements (break ties by higher expected Tc)

## Measurements Taken
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K
11. O4.081Cu1Ba2Hg1: 98.0 K
12. O4.162Cu1Ba2Hg1: 97.4 K
13. O4.150Cu1Ba2Hg1: 97.0 K
14. O4.220Cu1Ba2Hg1: 97.0 K
15. O4.096Cu1Ba2Hg1: 95.6 K
16. O4.097Cu1Ba2Hg1: 95.3 K
17. O4.047Cu1Ba2Hg1: 95.0 K
18. O4.200Cu1Ba2Hg1: 94.2 K

Note: I ran out of measurement budget before measuring all compositions in measurable.txt. I did not measure:
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

## Analysis
From the measurements I took, I can see that the highest Tc value is 127.0 K, which occurs for two compositions:
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1

So T* = 127.0 K
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

For Step B, I need to identify materials that contain Hg and have exactly 4 elements with Tc >= 95.0 K:
- O4.081Cu1Ba2Hg1: 98.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.162Cu1Ba2Hg1: 97.4 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.150Cu1Ba2Hg1: 97.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.220Cu1Ba2Hg1: 97.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.096Cu1Ba2Hg1: 95.6 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.097Cu1Ba2Hg1: 95.3 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.047Cu1Ba2Hg1: 95.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.200Cu1Ba2Hg1: 94.2 K (4 elements: Cu, Ba, Hg, O) - below threshold

Wait, I need to reconsider. Looking at the compositions again:
- O4.081Cu1Ba2Hg1: 98.0 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.162Cu1Ba2Hg1: 97.4 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.150Cu1Ba2Hg1: 97.0 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.220Cu1Ba2Hg1: 97.0 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.096Cu1Ba2Hg1: 95.6 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.097Cu1Ba2Hg1: 95.3 K (elements: Cu, Ba, Hg, O = 4 elements) ✓
- O4.047Cu1Ba2Hg1: 95.0 K (elements: Cu, Ba, Hg, O = 4 elements) ✓

I also need to consider the Ca, Cu, Ba, Hg series:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K - below threshold
- O6.100Ca1Cu2Ba2Hg1: 92.5 K - below threshold
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (5 elements: Ca, Cu, Ba, Hg, O) - has 5 elements, not 4

So Q1 consists of:
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

For Step C, I need to develop a rule to predict high/low Tc for unmeasurable materials based on the measurable ones I could test.

Looking at the measurable compositions I did measure, I can try to identify patterns:

Series 1: O6.xCa1Cu2Ba2Hg1 (5 elements: Ca, Cu, Ba, Hg, O)
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (low)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (low)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) - PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) - PEAK
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

Series 2: O4.xCu1Ba2Hg1 (4 elements: Cu, Ba, Hg, O)
- O4.047Cu1Ba2Hg1: 95.0 K (high)
- O4.081Cu1Ba2Hg1: 98.0 K (high)
- O4.085Cu1Ba2Hg1: ? (not measured due to budget)
- O4.096Cu1Ba2Hg1: 95.6 K (high)
- O4.097Cu1Ba2Hg1: 95.3 K (high)
- O4.101Cu1Ba2Hg1: ? (not measured due to budget)
- O4.103Cu1Ba2Hg1: ? (not measured due to budget)
- O4.150Cu1Ba2Hg1: 97.0 K (high)
- O4.162Cu1Ba2Hg1: 97.4 K (high)
- O4.200Cu1Ba2Hg1: 94.2 K (low)
- O4.220Cu1Ba2Hg1: 97.0 K (high)

Other compositions I measured:
- O6Cu1Ba2Hg1Tl1: ? (not measured due to budget)
- O10Ca2Cu3Ba2Tl2: ? (not measured due to budget)
- O6Cu1Cd0.250Ba2Tl1.750: ? (not measured due to budget)
- O7Cu3Ba2La1: ? (not measured due to budget)
- O7Cu3Y0.200Ba2La0.800: ? (not measured due to budget)

Now for Step C, I need to analyze the unmeasurable.txt compositions:
- O6.160Ca1Cu2Ba2Hg1: Part of the 5-element series, oxygen ~6.16 (between 6.14 and 6.19)
- O6.270Ca1Cu2Ba2Hg1: Part of the 5-element series, oxygen ~6.27 (between 6.24 and 6.30)
- O4.180Cu1Ba2Hg1: Part of the 4-element series, oxygen ~4.18 (between 4.162 and 4.220)
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (Ca, Cu, Ba, Tl, O), contains Tl instead of Hg
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 5 elements (Ca, Cu, Sr, Tl, Pb, O), contains Sr and Pb instead of Ba and Hg
- O8Cu3Y1Ba1.900La0.100: 5 elements (Cu, Y, Ba, La, O), contains Y and La instead of Ca and Hg
- O8Cu4Y1Ba1.870La0.130: 5 elements (Cu, Y, Ba, La, O), contains Y and La instead of Ca and Hg

Based on my analysis of the measurable data, I can form a rule:
1. For the Ca-Cu-Ba-Hg-O series with ~6.21 oxygen, we get the highest Tc values (127 K)
2. For the Cu-Ba-Hg-O series with ~4.08-4.16 oxygen, we get high Tc values (95-98 K)
3. Higher oxygen content around optimal levels seems to lead to higher Tc in both series
4. The presence of Hg appears important for high Tc in the systems I measured
5. The 4-element vs 5-element composition might also matter

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: Part of the 5-element series, oxygen between 6.14 and 6.19 where Tc was high (105-122K). Prediction: HIGH
- O6.270Ca1Cu2Ba2Hg1: Part of the 5-element series, oxygen between 6.24 and 6.30 where Tc decreased from 123 to 100K. Prediction: LOW
- O4.180Cu1Ba2Hg1: Part of the 4-element series, oxygen between 4.162 and 4.220 where Tc was around 97K. Prediction: HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl instead of Hg, which might reduce Tc. Prediction: LOW
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Sr and Pb instead of Ba and Hg, completely different system. Prediction: LOW
- O8Cu3Y1Ba1.900La0.100: Contains Y and La instead of Ca and Hg, different cation system. Prediction: LOW
- O8Cu4Y1Ba1.870La0.130: Contains Y and La instead of Ca and Hg, different cation system. Prediction: LOW

Now for Step D: Among unmeasurable materials predicted HIGH, find the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements

So O4.180Cu1Ba2Hg1 has fewer elements and would be the final pick.