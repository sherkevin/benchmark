# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Both search_db and measure_tc cost 1 unit each
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as O6.080Ca1Cu2Ba2Hg1.

First, I'll search for materials similar to the seed to identify the series.

## Results from Measurements
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K

Step A Result: The peak Tc is 127.0K, occurring at compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1.
Therefore, T* = 127.0K, and the peak composition is O6.210Ca1Cu2Ba2Hg1.

Step B: Need to find materials that contain Hg and have exactly 4 elements with Tc >= threshold1 = T* - 32 = 127.0 - 32 = 95.0K.

From my searches, I found materials like O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1 which have exactly 4 elements (O, Cu, Ba, Hg). Unfortunately, I ran out of budget to measure these materials.

Since I couldn't measure them due to budget constraints, I cannot identify which of these materials meet the criteria for Q1. However, I will assume that I would have measured them if I had budget remaining, and proceed with the rest of the analysis.

Step C: I need to predict high/low for unmeasurable materials based on a rule.
Unmeasurable materials I found: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1

From measurable materials in the series:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (high since >= 90K)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K (high since >= 90K)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K (high since >= 90K)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K (high since >= 90K)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K (high since >= 90K)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (high since >= 90K)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (high since >= 90K)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K (high since >= 90K)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K (high since >= 90K)

Looking at the pattern in the O6.x Ca1Cu2Ba2Hg1 series, Tc seems to peak around O6.210-O6.220, and decrease on either side. Based on this trend:
- O6.160Ca1Cu2Ba2Hg1: Likely high (between O6.140 with 105K and O6.190 with 122K, probably around 110-120K)
- O6.270Ca1Cu2Ba2Hg1: Likely high (between O6.240 with 123K and O6.300 with 100K, probably around 110-120K)

Step D: Among unmeasurable materials predicted high, choose the one with fewest elements (break ties by higher expected Tc).
Both O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 have the same number of elements (5: O, Ca, Cu, Ba, Hg), so I'd choose the one with higher expected Tc. Between them, O6.270 is closer to the peak (O6.270 vs O6.300 which measures 100K, while O6.160 is further from the peak), so O6.160 might be higher than O6.270 (closer to the peak at O6.210-O6.220).

Actually, let me reconsider: O6.160 is between O6.140 (105K) and the peak region, while O6.270 is between the peak (127K) and O6.300 (100K). So O6.160 might be around 110-115K and O6.270 might be around 110-120K. Since O6.270 is closer to the peak, it likely has a higher Tc than O6.160.

So the final pick would be O6.270Ca1Cu2Ba2Hg1.