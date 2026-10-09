# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units (shared between search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
- Need to find homologous doping series (same elements, varying oxygen)
- Find composition with maximum Tc (T*)
- Will start by measuring the seed material

## Step B: Filter Family B
- Materials containing Hg with exactly 4 elements
- Threshold1 = T* - 32
- Find all with Tc >= threshold1 (set Q1)

## Step C: Extrapolate
- Identify unmeasurable materials (no sample exists)
- Create prediction rule based on measurable materials
- Predict high/low for unmeasurable ones

## Step D: Final Pick
- From unmeasurable materials predicted as high
- Choose one with fewest elements (higher Tc as tiebreaker)

## Results
Step A: Measured all 12 members of the series O6.xCa1Cu2Ba2Hg1:
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K (PEAK - T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0K (PEAK - T*)
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K

So T* = 127.0K

Step B: Materials with Hg and exactly 4 elements:
- O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0K) - below threshold
- O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5K) - below threshold
- O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0K) - qualifies!
- O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0K) - qualifies!
- O6.160Ca1Cu2Ba2Hg1 (no sample, Tc = null) - can't evaluate
- O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0K) - qualifies!
- O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0K) - qualifies!
- O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0K) - qualifies!
- O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0K) - qualifies!
- O6.270Ca1Cu2Ba2Hg1 (no sample, Tc = null) - can't evaluate
- O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0K) - qualifies!
- O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0K) - qualifies!
- O6Cu1Ba2Hg1Tl1 (Tc = 0.0K) - below threshold

Threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Q1 = {O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1}

Step C: Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1: Based on trend (increasing toward peak at O6.21-O6.22), predicted HIGH
- O6.270Ca1Cu2Ba2Hg1: Based on trend (decreasing after peak), predicted HIGH

Step D: Both unmeasurable materials have 4 elements. Breaking tie by expected Tc: O6.270 should have higher Tc than O6.160 (since O6.270 is closer to the peak at O6.21-O6.22), so final answer is O6.270Ca1Cu2Ba2Hg1.

Final Answer File Created: answer.json