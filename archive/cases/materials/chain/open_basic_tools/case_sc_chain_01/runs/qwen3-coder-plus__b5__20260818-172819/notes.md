# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 26 units total
- Definition: high Tc >= 90K, low Tc < 90K
- Both measure_tc and search_db cost 1 budget unit each

## Step A - Series Peak
The seed belongs to a homologous doping series (same elements, varying oxygen).
Need to find the composition with maximum Tc (= T*).

First, let me measure the seed material to establish a baseline.
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K

Now I need to search for other materials in the same series (same elements, different oxygen amounts).

I searched for Hg-containing materials and found these series members:
Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
Page 4: O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
Page 5: O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O6.080Ca1Cu2Ba2Hg1 (seed)
Page 6: O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1
Page 7: O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1
Page 8: O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
Page 9: O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1, O6Cu1Ba2Hg1Tl1

I already measured the seed: O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K

Now I need to measure other materials in the series around this oxygen content to find the peak Tc.

Measurements completed:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (seed)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K

Step A - Series Peak:
The composition with maximum Tc is O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1, both with Tc = 127.0K.
So T* = 127.0K

Step B - Filter family B (contains Hg and has exactly 4 elements):
threshold1 = T* - 32 = 127.0 - 32 = 95.0K
I need to find materials that contain Hg and have exactly 4 elements, then report those with Tc >= 95.0K.

From my search results, the materials containing Hg were:
- O4.047Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.081Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.085Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.096Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.097Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.101Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.103Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.150Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.160Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.162Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.200Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O4.220Cu1Ba2Hg1 (5 elements: O, Cu, Ba, Hg)
- O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.100Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.130Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.140Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.190Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.210Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.220Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.240Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.300Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6.310Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
- O6Cu1Ba2Hg1Tl1 (6 elements: O, Cu, Ba, Hg, Tl)

Wait, looking more carefully at the compositions, all of the Ca-Cu-Ba-Hg materials have 5 elements (O, Ca, Cu, Ba, Hg), while the simpler ones like O4.047Cu1Ba2Hg1 have 4 elements (O, Cu, Ba, Hg).

Let me measure some of the 4-element materials: