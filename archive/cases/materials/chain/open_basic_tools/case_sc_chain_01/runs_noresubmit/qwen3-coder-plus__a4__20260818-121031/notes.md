# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 18 units total (for both measure_tc and search_db)
- Definition: high Tc >= 90K, low Tc < 90K
- Both tools share the same budget

## Step A: Series Peak
The seed belongs to a homologous doping series (same elements, varying oxygen).
Need to find series members and identify the composition with MAXIMUM Tc (= T*).

Series compositions and Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.160Ca1Cu2Ba2Hg1: no sample available
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K (highest Tc)
- O6.220Ca1Cu2Ba2Hg1: 127.0K (highest Tc)
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.270Ca1Cu2Ba2Hg1: no sample available
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K

Result: T* = 127.0K, occurring at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B: Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0K

From my search, materials containing Hg with exactly 4 elements:
- O4.047Cu1Ba2Hg1: Tc = 95.0K (>= 95.0, so it qualifies)
- O4.066Cu1Ba2Hg1: (not measured due to budget)
- O4.081Cu1Ba2Hg1: (not measured due to budget)
- O4.085Cu1Ba2Hg1: (not measured due to budget)
- O4.096Cu1Ba2Hg1: (not measured due to budget)
- O4.097Cu1Ba2Hg1: (not measured due to budget)

So Q1 includes O4.047Cu1Ba2Hg1, and potentially others if their Tc >= 95.0

## Step C: Extrapolate rule
Based on the series data, I can observe that Tc peaks around O6.21-O6.22 for the Ca1Cu2Ba2Hg1 series.
For materials without samples, I'll need to predict based on patterns.

Looking at the measured data:
- As oxygen content increases from O6.080 to O6.210-O6.220, Tc increases, peaking around 127.0K
- Then as oxygen continues to increase (O6.240, O6.300, O6.310), Tc decreases

For O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 (no samples available):
- O6.160 is between O6.140 (105.0K) and O6.190 (122.0K), so likely ~110-120K (high)
- O6.270 is between O6.240 (123.0K) and O6.300 (100.0K), so likely ~110-120K (high)

## Step D: Final pick
Need to find unmeasurable materials predicted as high, then select the one with fewest elements.

For now, based on my analysis, O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 are predicted to be high Tc.
Both have the same number of elements (5), so I'd pick the one expected to have higher Tc,
which would be O6.270Ca1Cu2Ba2Hg1 (~120K vs ~115K).