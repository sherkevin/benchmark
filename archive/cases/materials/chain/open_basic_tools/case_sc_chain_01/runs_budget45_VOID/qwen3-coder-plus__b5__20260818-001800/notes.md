# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 45 units total (for both search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
Found materials in the series OxCu1Ba2Hg1 and OxCa1Cu2Ba2Hg1:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Tc = null (no sample)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc!)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc!)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Tc = null (no sample)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
- O4.081Cu1Ba2Hg1: Tc = 98.0 K
- O4.085Cu1Ba2Hg1: Tc = 92.0 K
- O4.101Cu1Ba2Hg1: Tc = 93.4 K

So T* = 127.0 K for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B: Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0
Materials containing Hg with exactly 4 elements that have Tc >= 95.0:
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥ 95.0) ✓
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95.0) ✗
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95.0) ✗

So Q1 = ["O4.081Cu1Ba2Hg1"]

## Step C & D: Extrapolate and Final Pick
Materials without samples (unmeasurable):
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1

Based on the pattern I observed, materials tend to have higher Tc when the oxygen content is around the optimal value (around 6.21). I'll make predictions based on the relationship between oxygen content and Tc in the measurable materials:

Rule: For materials with formula OxCu1Ba2Hg1, Tc increases as x approaches ~4.18-4.20, then decreases. For materials with formula OxCa1Cu2Ba2Hg1, Tc peaks around x=6.21-6.22.

For unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: This is before the peak (6.21-6.22), likely high Tc → high
- O6.270Ca1Cu2Ba2Hg1: This is after the peak, Tc decreasing → low
- O4.047Cu1Ba2Hg1: Close to O4.081Cu1Ba2Hg1 which has Tc=98, likely high → high
- O4.066Cu1Ba2Hg1: Close to O4.081Cu1Ba2Hg1 which has Tc=98, likely high → high

Step D: Among unmeasurable materials predicted high, O4.047Cu1Ba2Hg1 and O4.066Cu1Ba2Hg1 have 4 elements each. O6.160Ca1Cu2Ba2Hg1 has 5 elements. So the one with fewest elements would be either O4.047Cu1Ba2Hg1 or O4.066Cu1Ba2Hg1. Since O4.066 is closer to O4.081 which measured 98K, I'll predict slightly higher Tc for O4.066Cu1Ba2Hg1.

Final pick: O4.066Cu1Ba2Hg1