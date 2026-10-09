# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: High Tc >= 90 K, otherwise Low
- Measurement budget: 34 calls to measure_tc

## Step A - Series Peak
Results from CaCu2Ba2Hg1 series (O6.xx):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

Results from Cu1Ba2Hg1 series (O4.xx):
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.180Cu1Ba2Hg1: No sample (unmeasurable)
- O4.047Cu1Ba2Hg1: No sample (unmeasurable)
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

Other measurable materials:
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

Maximum Tc = 127.0 K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0 K, peak composition is O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)

## Step B - Filter Family B
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Find materials containing Hg and having exactly 4 elements with Tc >= threshold1
- Call this set Q1

Materials containing Hg with exactly 4 elements:
- O4.081Cu1Ba2Hg1: Tc = 98.0 K ≥ 95.0 K → qualifies
- O4.162Cu1Ba2Hg1: Tc = 97.4 K ≥ 95.0 K → qualifies
- O4.150Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K → qualifies
- O4.220Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K → qualifies
- O4.096Cu1Ba2Hg1: Tc = 95.6 K ≥ 95.0 K → qualifies
- O4.097Cu1Ba2Hg1: Tc = 95.3 K ≥ 95.0 K → qualifies
- O4.200Cu1Ba2Hg1: Tc = 94.2 K < 95.0 K → does not qualify
- O4.101Cu1Ba2Hg1: Tc = 93.4 K < 95.0 K → does not qualify
- O4.103Cu1Ba2Hg1: Tc = 92.5 K < 95.0 K → does not qualify
- O4.085Cu1Ba2Hg1: Tc = 92.0 K < 95.0 K → does not qualify

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}

## Step C - Extrapolate
- Materials in `data/unmeasurable.txt` cannot be measured
- Need to induce a rule to separate high from low Tc based on measurable materials
- Apply rule to predict high/low for unmeasurable materials
- Unmeasurable materials: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130

Analyzing measurable materials to find patterns:
High Tc (≥ 90 K): 
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
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K

Low Tc (< 90 K):
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

Patterns identified:
1. For O6.xCa1Cu2Ba2Hg1 series: Peak Tc around O6.21-O6.22, dropping off on either side
2. For O4.xCu1Ba2Hg1 series (4 elements): Consistently high Tc (92-98K)
3. Materials containing La or Y tend to be low Tc
4. Complex materials with multiple heavy elements (like Tl, Cd) tend to be low Tc, though exceptions exist

Induced Rule:
A material is high Tc if:
1. It belongs to the Ca-Cu-Ba-Hg-O family with oxygen content around 6.13-6.24 (optimal ~6.21-6.22), OR
2. It belongs to the Cu-Ba-Hg-O family with 4 elements total (O4.x range), OR
3. It has a simpler composition without La, Y, or complex mixtures of heavy elements

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: In Ca-Cu-Ba-Hg-O family, x=6.16 (between 6.13-6.24) → HIGH
- O6.270Ca1Cu2Ba2Hg1: In Ca-Cu-Ba-Hg-O family, x=6.27 (>6.24 but still in reasonable range) → HIGH (though lower than peak)
- O4.180Cu1Ba2Hg1: 4 elements, O4.x family → HIGH
- O4.047Cu1Ba2Hg1: 4 elements, O4.x family → HIGH (part of successful 4-element system)
- O4.066Cu1Ba2Hg1: 4 elements, O4.x family → HIGH (part of successful 4-element system)
- O4.160Cu1Ba2Hg1: 4 elements, O4.x family → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl and is complex → LOW
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl, Pb, Sr (complex) → LOW
- O8Cu3Y1Ba1.900La0.100: Contains Y, La → LOW
- O8Cu4Y1Ba1.870La0.130: Contains Y, La → LOW

## Step D - Final Pick
Among unmeasurable materials predicted as high: 
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)

Materials with fewest elements (4): O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1

Breaking ties by expected Tc (based on measured O4.xCu1Ba2Hg1 series pattern):
From measured data, Tc values in the 4-element series:
- O4.081: 98.0 K (highest measured)
- O4.162: 97.4 K
- O4.150: 97.0 K
- O4.220: 97.0 K
- O4.096: 95.6 K
- O4.180: (unmeasurable, would be estimated based on proximity to measured values)
- O4.160: (close to O4.162, likely ~97.4 K)
- O4.066: (closer to O4.081 peak, potentially high)
- O4.047: (lower O content, possibly lower than peak range)
- O4.200: 94.2 K

O4.160 is closest to the well-performing O4.162 (97.4K), so it would likely have high Tc.
O4.066 is close to the peak performer O4.081, so it would likely have high Tc.
O4.180 is close to O4.200 (94.2K) and O4.220 (97.0K), so might be in the middle.
O4.047 is at the low end of O content, possibly lower Tc.

Between O4.066, O4.160 (both likely high Tc), I'll go with O4.160Cu1Ba2Hg1 as it's closer to a known high performer (O4.162).

Final answer: O4.160Cu1Ba2Hg1