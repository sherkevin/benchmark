# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total
- Definition: high Tc >= 90K, low Tc < 90K
- Both measure_tc and search_db cost 1 budget unit each

## Step A: Series Peak
- Need to find materials in the same homologous doping series (same elements, varying oxygen)
- Find the composition with maximum Tc (T*)
- Seed material O6.080Ca1Cu2Ba2Hg1 has Tc = 92.0K
- Measured compositions in the series O6.xCa1Cu2Ba2Hg1:
  - O6.080Ca1Cu2Ba2Hg1: 92.0K
  - O6.100Ca1Cu2Ba2Hg1: 92.5K
  - O6.130Ca1Cu2Ba2Hg1: 102.0K
  - O6.140Ca1Cu2Ba2Hg1: 105.0K
  - O6.160Ca1Cu2Ba2Hg1: No sample available
  - O6.190Ca1Cu2Ba2Hg1: 122.0K
  - O6.210Ca1Cu2Ba2Hg1: 127.0K (MAX!)
  - O6.220Ca1Cu2Ba2Hg1: 127.0K (MAX!)
  - O6.240Ca1Cu2Ba2Hg1: 123.0K
  - O6.270Ca1Cu2Ba2Hg1: No sample available
  - O6.300Ca1Cu2Ba2Hg1: 100.0K
  - O6.310Ca1Cu2Ba2Hg1: 98.0K
- T* = 127.0K (for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

## Step B: Filter family B (contains Hg and has exactly 4 elements)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0K
- Materials containing Hg with exactly 4 elements (O, Cu, Ba, Hg):
  - O4.047Cu1Ba2Hg1: No sample available
  - O4.066Cu1Ba2Hg1: No sample available
  - O4.081Cu1Ba2Hg1: 98.0K >= 95.0K ✓
  - O4.085Cu1Ba2Hg1: 92.0K < 95.0K
  - O4.096Cu1Ba2Hg1: 95.6K >= 95.0K ✓
  - O4.097Cu1Ba2Hg1: 95.3K >= 95.0K ✓
  - O4.101Cu1Ba2Hg1: 93.4K < 95.0K
  - O4.103Cu1Ba2Hg1: 92.5K < 95.0K
  - O4.150Cu1Ba2Hg1: 97.0K >= 95.0K ✓
  - O4.160Cu1Ba2Hg1: No sample available
  - O4.162Cu1Ba2Hg1: 97.4K >= 95.0K ✓
  - O4.180Cu1Ba2Hg1: No sample available
  - O4.200Cu1Ba2Hg1: 94.2K < 95.0K
  - O4.220Cu1Ba2Hg1: 97.0K >= 95.0K ✓

- Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C: Extrapolate (predict for unmeasurable materials)
- Unmeasurable materials I found:
  - O6.160Ca1Cu2Ba2Hg1: No sample available - PREDICTION: Based on the trend (peaking around O6.210-O6.220), this is likely lower Tc, so LOW
  - O6.270Ca1Cu2Ba2Hg1: No sample available - PREDICTION: After the peak, Tc decreases, so LOW
  - O4.047Cu1Ba2Hg1: No sample available - PREDICTION: Lower oxygen content than O4.081 (which had 98.0K), likely HIGH
  - O4.066Cu1Ba2Hg1: No sample available - PREDICTION: Similar to O4.047, likely HIGH
  - O4.160Cu1Ba2Hg1: No sample available - PREDICTION: Similar to O4.162 (97.4K), likely HIGH
  - O4.180Cu1Ba2Hg1: No sample available - PREDICTION: Between O4.162(97.4K) and O4.200(94.2K), likely HIGH

Other materials in database:
  - O10Ca2Cu3Ba2Tl2
  - O6Cu1Ba2Hg1Tl1  
  - O6Cu1Cd0.250Ba2Tl1.750
  - O7Ca1Cu2Sr2Tl0.500Pb0.500
  - O8Ca0.850Cu2.200Ba2Tl1.800

Let me try to measure some of these:
  - O10Ca2Cu3Ba2Tl2: Try to measure
  - O6Cu1Ba2Hg1Tl1: Try to measure
  - O6Cu1Cd0.250Ba2Tl1.750: Try to measure
  - O7Ca1Cu2Sr2Tl0.500Pb0.500: Try to measure
  - O8Ca0.850Cu2.200Ba2Tl1.800: Try to measure

Wait, I've used up my budget. I cannot measure these additional materials.

Rule for prediction based on available data:
Looking at the trends in oxygen content vs Tc:
- For O6.xCa1Cu2Ba2Hg1 series: Tc peaks around O6.210-O6.220 (127.0K) and decreases on either side
- For O4.xCu1Ba2Hg1 series: Higher oxygen content seems to generally lead to higher Tc up to a point
- General observation: Oxygen stoichiometry affects Tc significantly

My prediction rule: Materials with intermediate oxygen content in known series tend to have higher Tc values due to optimal doping levels. Very low or very high oxygen content typically results in lower Tc.

## Step D: Final pick
- Among unmeasurable materials predicted high, choose the one with fewest elements
- Predicted HIGH unmeasurable materials:
  - O4.047Cu1Ba2Hg1 (4 elements): HIGH
  - O4.066Cu1Ba2Hg1 (4 elements): HIGH
  - O4.160Cu1Ba2Hg1 (4 elements): HIGH
  - O4.180Cu1Ba2Hg1 (4 elements): HIGH

All of these have 4 elements, so I'll pick the one I expect to have the highest Tc. Based on the trend that values closer to O4.081 (98.0K) and O4.162 (97.4K) are high, O4.047Cu1Ba2Hg1 might be high, but it's at the lower end. O4.066 is close to O4.081 which measured 98.0K, so this would be my prediction for the highest Tc among the unmeasurable ones.