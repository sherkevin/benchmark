# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units total (for both measure_tc and search_db)
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen). Need to find and measure series members to identify the composition with MAXIMUM Tc (= T*).

Found all 12 materials in the series through 6 pages of search:
- Page 1: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1
- Page 2: O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1
- Page 3: O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1
- Page 4: O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1
- Page 5: O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
- Page 6: O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measurements:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) = 127.0 K, achieved by:
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1

So the peak composition is O6.210Ca1Cu2Ba2Hg1.

## Step B - Filter Family B
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find materials that contain Hg and have exactly 4 elements, with Tc >= 95.0 K.

From the Hg-containing materials search, the 4-element materials (with Hg) are:
- O6.080Ca1Cu2Ba2Hg1 - Tc = 92.0 K (< 95.0 K) - doesn't qualify
- O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (< 95.0 K) - doesn't qualify
- O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (>= 95.0 K) - qualifies
- O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (>= 95.0 K) - qualifies
- O6.160Ca1Cu2Ba2Hg1 - No sample available
- O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (>= 95.0 K) - qualifies
- O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (>= 95.0 K) - qualifies
- O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (>= 95.0 K) - qualifies
- O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (>= 95.0 K) - qualifies
- O6.270Ca1Cu2Ba2Hg1 - No sample available
- O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (>= 95.0 K) - qualifies
- O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K (>= 95.0 K) - qualifies
- O6Cu1Ba2Hg1Tl1 - Tc = 0.0 K (< 95.0 K) - doesn't qualify

Q1 = {O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1}

## Step C - Extrapolate (no measurement possible)
Unmeasurable materials I discovered via search_db:
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.270Ca1Cu2Ba2Hg1: No sample available

Rule induction from measurable materials in the series:
Looking at the relationship between oxygen content and Tc, I notice a bell-shaped curve with peak Tc around oxygen content 6.21-6.22.
Based on the interpolation between neighboring compositions:
- O6.160Ca1Cu2Ba2Hg1: likely has Tc around 115 K (between 105.0 and 122.0), so "high"
- O6.270Ca1Cu2Ba2Hg1: likely has Tc around 115 K (between 123.0 and 100.0), so "high"

## Step D - Final pick
Among unmeasurable materials predicted "high", I have:
- O6.160Ca1Cu2Ba2Hg1: 4 elements
- O6.270Ca1Cu2Ba2Hg1: 4 elements

Both have 4 elements, so I need to break the tie by higher expected Tc. Both are predicted to have similar Tc (~115K), so I can choose either. I'll choose O6.160Ca1Cu2Ba2Hg1 as the final answer.