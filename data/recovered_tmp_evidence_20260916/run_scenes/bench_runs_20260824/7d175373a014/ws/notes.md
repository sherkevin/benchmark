# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- Step A: Find maximum Tc in homologous doping series (same element set, varying oxygen)
- Step B: Find all materials with exactly 4 elements containing Hg with Tc >= threshold1 (T* - 32)
- Step C: Predict high/low for unmeasurable materials (7 compositions)
- Step D: Pick unmeasurable material predicted high with fewest elements

## Initial Observations
1. Measurable materials (27 total):
   - O6.xxxCa1Cu2Ba2Hg1 series (10 materials) - homologous doping series with varying oxygen
   - O4.xxxCu1Ba2Hg1 series (11 materials) - another series
   - Other materials (6 materials) - various compositions

2. Unmeasurable materials (7 total):
   - O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1 - continuation of O6 series
   - O4.180Cu1Ba2Hg1 - continuation of O4 series
   - 4 other complex materials

## Measurement Strategy
I need to be efficient with my 18 measurement budget. Let me start with Step A.

## Step A Measurements (O6 Series)
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc in O6 series: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Budget used: 10/18

Step A Analysis:
- The seed O6.080Ca1Cu2Ba2Hg1 belongs to homologous series O6.xxxCa1Cu2Ba2Hg1
- I've measured all 10 members of this series
- Maximum Tc = 127.0 K
- This appears to be T* for Step A

Step B Analysis:
- Threshold1 = T* - 32 = 127 - 32 = 95 K
- Need to find materials containing Hg with exactly 4 elements and Tc >= 95 K
- O4.xxxCu1Ba2Hg1 series (11 materials) qualifies (4 elements: O, Cu, Ba, Hg)

O4 Series Measurements (8/11 measured):
1. O4.047Cu1Ba2Hg1: Tc = 95.0 K (>= 95) ✓
2. O4.081Cu1Ba2Hg1: Tc = 98.0 K (>= 95) ✓
3. O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95) ✗
4. O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95) ✗
5. O4.150Cu1Ba2Hg1: Tc = 97.0 K (>= 95) ✓
6. O4.162Cu1Ba2Hg1: Tc = 97.4 K (>= 95) ✓
7. O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95) ✗
8. O4.220Cu1Ba2Hg1: Tc = 97.0 K (>= 95) ✓

Unmeasured O4 materials (3):
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1

Budget: 18/18 used. I cannot measure the remaining 3 O4 materials.

For Step C, I need to induce a rule from measurable materials to predict unmeasurable ones. I have measurements for:
- O6 series (10 materials) - all high Tc (>= 90 K)
- O4 series (8 materials) - mixed high/low

Missing measurements:
- Other materials in measurable.txt (6 materials, some without Hg)
- 3 O4 series materials

Problem: I haven't measured any materials without Hg, which might be important for understanding the rule.

Step C Analysis:
Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - continuation of O6 series, likely high
2. O6.270Ca1Cu2Ba2Hg1 - continuation of O6 series, likely high but decreasing
3. O4.180Cu1Ba2Hg1 - between O4.162 (97.4K high) and O4.200 (94.2K low), borderline
4. O8Ca0.850Cu2.200Ba2Tl1.800 - complex, no similar measurements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - complex, no similar measurements
6. O8Cu3Y1Ba1.900La0.100 - complex, no similar measurements
7. O8Cu4Y1Ba1.870La0.130 - complex, no similar measurements

I need to induce a rule based on my limited data. Looking at patterns:
- O6 series: All high, contains Hg, 5 elements, has Ca and Ba
- O4 series: Mixed, contains Hg, 4 elements, has Ba but no Ca

Possible rule: Materials are HIGH Tc if they contain BOTH:
1. At least one alkaline earth metal (Ca, Ba, Sr) AND
2. At least one heavy post-transition metal (Hg, Tl, Pb) AND
3. Appropriate stoichiometry (needs more data)

But O4 series has Ba (alkaline earth) and Hg (heavy metal), yet mixed results. So maybe need specific combinations.

Alternative rule: Materials are HIGH Tc if they contain Ca AND Hg specifically. O6 has both, all high. O4 has Hg but no Ca, mixed.

But material 4 (O8Ca0.850Cu2.200Ba2Tl1.800) has Ca and Tl (similar to Hg). If Ca + heavy metal rule holds, this might be high.

Material 5 has Ca, Tl, Pb - might be high.
Materials 6-7 have no Ca or heavy metals - might be low.

## Final Analysis and Answer

Step A:
- T* = 127.0 K (maximum in O6 series)
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127 K)

Step B:
- Threshold1 = 95.0 K
- Qualifying materials (O4 series with Tc >= 95 K): O4.047, O4.081, O4.150, O4.162, O4.220
- Note: 3 O4 materials unmeasured (O4.096, O4.097, O4.103) - not included in answer

Step C Predictions:
Key insight: All O4 materials measured have Tc >= 92.0 K, so all are HIGH (>= 90 K). The distinction for Step B (>= 95 K) is different from HIGH/LOW classification.
1. O6.160Ca1Cu2Ba2Hg1: HIGH (has Hg)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (has Hg)
3. O4.180Cu1Ba2Hg1: HIGH (has Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (has Ca and Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (has Ca, Tl, Pb)
6. O8Cu3Y1Ba1.900La0.100: LOW (no Hg or Ca+heavy metal)
7. O8Cu4Y1Ba1.870La0.130: LOW (no Hg or Ca+heavy metal)

Induced Rule: "Materials are HIGH Tc if they contain mercury (Hg) OR contain calcium (Ca) together with thallium (Tl) or lead (Pb)."

Step D Final Pick:
- Among predicted HIGH materials: O6.160 (5 elements), O6.270 (5), O4.180 (4), O8Ca0.850... (5), O7Ca1Cu2Sr2Tl0.500Pb0.500 (6)
- Fewest elements: O4.180 has 4 elements
- O4.180 has fewer elements than others (4 vs 5+), so it's not a tie
- Therefore final pick: O4.180Cu1Ba2Hg1

Limitations:
- Used all 18 measurements without optimal strategy
- Didn't measure non-Hg materials, limiting rule induction
- O4.180 has 4 elements (fewer than O6.160), but expected Tc lower
- Followed "break ties by higher expected Tc" to pick O6.160 over O4.180