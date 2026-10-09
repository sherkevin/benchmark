# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (shared between search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
Need to find the homologous doping series with the same elements as the seed (O, Ca, Cu, Ba, Hg) but varying oxygen amounts.

The seed material O6.080Ca1Cu2Ba2Hg1 has Tc = 92.0K.

## Step A: Series Peak
Results of Tc measurements for the series:

O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
O6.160Ca1Cu2Ba2Hg1: No sample available
O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (MAXIMUM Tc)
O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (MAXIMUM Tc)
O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
O6.270Ca1Cu2Ba2Hg1: No sample available
O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Peak composition: O6.210Ca1Cu2Ba2Hg1 (and O6.220Ca1Cu2Ba2Hg1 both have Tc = 127.0K)
Maximum Tc (T*) = 127.0K

## Step B: Filter Family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Need to find materials that contain Hg and have exactly 4 elements, then report ALL whose Tc >= 95.0K.

Materials with exactly 4 elements (containing Hg):
- O6.080Ca1Cu2Ba2Hg1 (already measured: Tc = 92.0K)
- O6.100Ca1Cu2Ba2Hg1 (already measured: Tc = 92.5K)
- O6.130Ca1Cu2Ba2Hg1 (already measured: Tc = 102.0K)
- O6.140Ca1Cu2Ba2Hg1 (already measured: Tc = 105.0K)
- O6.160Ca1Cu2Ba2Hg1 (already measured: no sample available)
- O6.190Ca1Cu2Ba2Hg1 (already measured: Tc = 122.0K)
- O6.210Ca1Cu2Ba2Hg1 (already measured: Tc = 127.0K)
- O6.220Ca1Cu2Ba2Hg1 (already measured: Tc = 127.0K)
- O6.240Ca1Cu2Ba2Hg1 (already measured: Tc = 123.0K)
- O6.270Ca1Cu2Ba2Hg1 (already measured: no sample available)
- O6.300Ca1Cu2Ba2Hg1 (already measured: Tc = 100.0K)
- O6.310Ca1Cu2Ba2Hg1 (already measured: Tc = 98.0K)
- O6Cu1Ba2Hg1Tl1 (need to measure)

From the already measured materials with 4 elements:
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K >= 95.0K ✓
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K >= 95.0K ✓
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K >= 95.0K ✓
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K >= 95.0K ✓
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K >= 95.0K ✓
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K >= 95.0K ✓
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K >= 95.0K ✓
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K >= 95.0K ✓

O6Cu1Ba2Hg1Tl1 has Tc = 0.0K, which is less than 95.0K, so it doesn't qualify.

## Step B Results:
Materials containing Hg and having exactly 4 elements with Tc >= 95.0K (Q1):
- O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0K)
- O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0K)
- O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0K)
- O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0K)
- O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0K)
- O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0K)
- O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0K)
- O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0K)

Threshold: 95.0K

## Step C: Extrapolate (no measurement possible)

Materials I have measured:
High Tc (>= 90K):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Low Tc (< 90K):
- O6Cu1Ba2Hg1Tl1: Tc = 0.0K

Materials that are not measurable (no sample available):
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

Looking at the pattern, I notice that oxygen content might be a factor. In the series O6.080Ca1Cu2Ba2Hg1 to O6.310Ca1Cu2Ba2Hg1, Tc generally increases with oxygen content up to an optimal point around O6.210-O6.220, then decreases. The two unmeasurable materials in this series are O6.160 and O6.270. 

O6.160 has lower oxygen content than the peak, and O6.270 has higher oxygen content than the peak. Based on the trend in the series, I can predict:
- O6.160Ca1Cu2Ba2Hg1 would likely have a Tc lower than the peak (probably lower than 90K, making it low)
- O6.270Ca1Cu2Ba2Hg1 would have a Tc lower than the peak but still potentially high (>90K) since it's still close to the peak range

However, looking more carefully at the values near the peak:
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (peak)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (peak)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: no sample (would likely be lower Tc, possibly still high)

Rule for predicting high/low:
Based on the oxygen content in the series, materials with oxygen content around 6.19-6.24 appear to have high Tc (>= 90K), while those with significantly lower or higher oxygen content tend to have lower Tc. So I'll predict:
- O6.160Ca1Cu2Ba2Hg1: low (too low oxygen content compared to optimal range)
- O6.270Ca1Cu2Ba2Hg1: high (still within reasonable range of the optimal, though decreasing)

## Step D: Final pick
Among unmeasurable materials predicted to be high, choose the one with the fewest elements (break ties by higher expected Tc).

Unmeasurable materials predicted to be high:
- O6.270Ca1Cu2Ba2Hg1: Elements = Ca, Cu, Ba, Hg = 4 elements

There's only one predicted to be high, so:
Final answer for Step D: O6.270Ca1Cu2Ba2Hg1