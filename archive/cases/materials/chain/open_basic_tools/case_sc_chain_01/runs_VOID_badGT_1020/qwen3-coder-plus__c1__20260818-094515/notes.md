# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units total
- Definition: high Tc >= 90K, low Tc < 90K
- Both measure_tc and search_db cost 1 budget unit each

## Step A: Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as O6.080Ca1Cu2Ba2Hg1 and determine which has the maximum Tc.

Found 12 materials in the series:
- O6.080Ca1Cu2Ba2Hg1 (Tc: 92.0K)
- O6.100Ca1Cu2Ba2Hg1 (Tc: 92.5K)
- O6.130Ca1Cu2Ba2Hg1 (Tc: 102.0K)
- O6.140Ca1Cu2Ba2Hg1 (Tc: 105.0K)
- O6.160Ca1Cu2Ba2Hg1 (no sample available)
- O6.190Ca1Cu2Ba2Hg1 (Tc: 122.0K)
- O6.210Ca1Cu2Ba2Hg1 (Tc: 127.0K) <- PEAK
- O6.220Ca1Cu2Ba2Hg1 (Tc: 127.0K) <- PEAK
- O6.240Ca1Cu2Ba2Hg1 (Tc: 123.0K)
- O6.270Ca1Cu2Ba2Hg1 (no sample available)
- O6.300Ca1Cu2Ba2Hg1 (Tc: 100.0K)
- O6.310Ca1Cu2Ba2Hg1 (Tc: 98.0K)

Maximum Tc (T*) = 127.0K
Peak compositions: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B: Filter Family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Need to find materials that contain Hg and have exactly 4 elements, then report those with Tc >= 95.0K.

Materials with exactly 4 elements containing Hg:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (< 95.0K) - not in Q1
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K (< 95.0K) - not in Q1
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K (>= 95.0K) - in Q1
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K (>= 95.0K) - in Q1
- O6.160Ca1Cu2Ba2Hg1: no sample available - not in Q1
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K (>= 95.0K) - in Q1
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (>= 95.0K) - in Q1
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (>= 95.0K) - in Q1
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K (>= 95.0K) - in Q1
- O6.270Ca1Cu2Ba2Hg1: no sample available - not in Q1
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K (>= 95.0K) - in Q1
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K (>= 95.0K) - in Q1
- O6Cu1Ba2Hg1Tl1: Tc = 0.0K (< 95.0K) - not in Q1

Qualifying materials (Q1): O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

## Step C: Extrapolate (no measurement possible)
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1: Based on interpolation between O6.140 (Tc=105.0K) and O6.190 (Tc=122.0K), this would likely have high Tc (~110-115K). Prediction: high
- O6.270Ca1Cu2Ba2Hg1: Based on interpolation between O6.240 (Tc=123.0K) and O6.300 (Tc=100.0K), this would likely have high Tc (~110-115K). Prediction: high

Rule: Based on the oxygen content in the Ca1Cu2Ba2Hg1 series, materials with oxygen content around 6.130-6.310 (excluding the lowest values around 6.080-6.100) tend to have high Tc values above 90K, with a peak around 6.210-6.220.

## Step D: Final Pick
Unmeasurable materials predicted to be high:
- O6.160Ca1Cu2Ba2Hg1 (4 elements, predicted Tc ~110-115K)
- O6.270Ca1Cu2Ba2Hg1 (4 elements, predicted Tc ~110-115K)

Both have 4 elements, so tie goes to the one with higher expected Tc. Between O6.160 (~110-115K) and O6.270 (~110-115K), I estimate O6.160 to be slightly higher since it's closer to the peak region. Final choice: O6.160Ca1Cu2Ba2Hg1