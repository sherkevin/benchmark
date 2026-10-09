# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 34 calls to measure_tc
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
Measured all compositions in data/measurable.txt:
- Maximum Tc = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 (both have 127.0 K)

## Step B - Filter Family B
- Contains Hg and has exactly 4 elements
- threshold1 = T* - 32 = 127.0 - 32 = 95.0
- Find all materials with Tc >= threshold1
- Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Extrapolate
- Predict high/low for unmeasurable materials
- Need to induce a rule based on measurable materials

Rule: Materials containing Tl, La, or Y tend to have low Tc (< 90K), while materials containing Hg, Ca, Cu, and Ba (without Tl/La/Y) tend to have high Tc (>= 90K).

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Contains Ca, Cu, Ba, Hg, O - Similar to high Tc materials in the same series - PREDICTED HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Ca, Cu, Ba, Hg, O - Similar to high Tc materials in the same series - PREDICTED HIGH
3. O4.180Cu1Ba2Hg1: Contains Cu, Ba, Hg, O - Similar to high Tc 4-element Hg materials - PREDICTED HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ca, Cu, Ba, Tl, O - Contains Tl which correlates with low Tc - PREDICTED LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ca, Cu, Sr, Tl, Pb, O - Contains Tl which correlates with low Tc - PREDICTED LOW
6. O8Cu3Y1Ba1.900La0.100: Contains Cu, Y, Ba, La, O - Contains Y and La which correlate with low Tc - PREDICTED LOW
7. O8Cu4Y1Ba1.870La0.130: Contains Cu, Y, Ba, La, O - Contains Y and La which correlate with low Tc - PREDICTED LOW

## Step D - Final Pick
- From unmeasurable high predictions, pick the one with fewest elements
- High predictions: O6.160Ca1Cu2Ba2Hg1 (5 elements), O6.270Ca1Cu2Ba2Hg1 (5 elements), O4.180Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 has the fewest elements (4), so this is the final pick