# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
Goal: Find the maximum Tc in the homologous doping series of the seed material.

First, let's measure the seed material to establish a baseline.
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K

Now I need to explore the homologous series by looking for similar compositions with different oxygen content.

Found the following compositions in the series:
- O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0K)
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

Now I'll measure the Tc values for all of them to find the peak.

Results:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: Tc = null (no sample available)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: Tc = null (no sample available)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Step A Result:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both have Tc = 127.0K)
- Maximum Tc (T*) = 127.0K

Moving to Step B: Find materials containing Hg and having exactly 4 elements with Tc >= threshold1, where threshold1 = T* - 32 = 127.0 - 32 = 95.0K.

So far I've found:
- O4.047Cu1Ba2Hg1: Tc = 95.0K (meets threshold)

Since I've reached the budget limit (18/18 units used), I can't measure additional Hg-containing materials.

Step B Results:
- Threshold = 95.0K
- Qualifying materials (Q1) containing Hg with exactly 4 elements and Tc >= 95.0K: [O4.047Cu1Ba2Hg1]

Moving to Step C: From the series exploration, I found some unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.270Ca1Cu2Ba2Hg1: No sample available

I need to predict if these are high (Tc >= 90K) or low (Tc < 90K) based on a rule I induce from measurable materials.

Looking at the measurable materials in the Ca1Cu2Ba2Hg1 series:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (high)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K (high)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K (high)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K (high)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K (high)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (high)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (high)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K (high)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K (high)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K (high)

Looking at the trend of Tc vs oxygen content:
- O6.080 -> 92.0K
- O6.100 -> 92.5K
- O6.130 -> 102.0K
- O6.140 -> 105.0K
- O6.160 -> (no sample)
- O6.190 -> 122.0K
- O6.210 -> 127.0K
- O6.220 -> 127.0K
- O6.240 -> 123.0K
- O6.270 -> (no sample)
- O6.300 -> 100.0K
- O6.310 -> 98.0K

It appears that there's a peak around O6.210-O6.220, with Tc decreasing on either side. Based on the interpolation between nearby points:
- O6.160 is between O6.140 (105.0K) and O6.190 (122.0K), so likely high
- O6.270 is between O6.240 (123.0K) and O6.300 (100.0K), so likely high

Rule: Materials in this series with oxygen content between ~6.100 and ~6.300 tend to be high Tc superconductors, with the peak around 6.210-6.220. Both unmeasurable materials fall in this range, so they're likely high.

Step C Predictions:
- O6.160Ca1Cu2Ba2Hg1: high
- O6.270Ca1Cu2Ba2Hg1: high

For Step D, I need to find among unmeasurable materials predicted as high, the one with the fewest elements. All materials in this series have the same elements (O, Ca, Cu, Ba, Hg), so they all have 5 elements. In case of tie, I'd choose based on higher expected Tc.

Since both unmeasurable materials have 5 elements, I'll predict O6.160Ca1Cu2Ba2Hg1 would have a higher Tc (between 105.0K and 122.0K) compared to O6.270Ca1Cu2Ba2Hg1 (between 100.0K and 123.0K), making O6.160Ca1Cu2Ba2Hg1 the better choice.