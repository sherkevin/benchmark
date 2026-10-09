# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90K, Low Tc < 90K
- Budget: 34 units total
- Tools: measure_tc(composition) and search_db(query, page)

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find series members to identify the composition with MAXIMUM Tc (= T*).

First, I'll measure the seed material to establish a baseline.
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (measured)

Found materials in the series:
Page 1: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1
Page 2: O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1
Page 3: O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1
Page 4: O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measurement results:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: Tc = no sample (not measurable)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: Tc = no sample (not measurable)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Step A - Series Peak:
Maximum Tc = 127.0K (both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
So T* = 127.0K

## Step B - Filter Family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= 95.0.

Materials with Hg and exactly 4 elements (Cu-Ba-Hg-O):
- O4.047Cu1Ba2Hg1: no sample
- O4.066Cu1Ba2Hg1: no sample
- O4.081Cu1Ba2Hg1: Tc = 98.0K (>= 95.0, qualifies)
- O4.085Cu1Ba2Hg1: Tc = 92.0K (< 95.0, doesn't qualify)
- O4.096Cu1Ba2Hg1: Tc = 95.6K (>= 95.0, qualifies)
- O4.097Cu1Ba2Hg1: Tc = 95.3K (>= 95.0, qualifies)
- O4.101Cu1Ba2Hg1: Tc = 93.4K (< 95.0, doesn't qualify)
- O4.103Cu1Ba2Hg1: Tc = 92.5K (< 95.0, doesn't qualify)
- O4.150Cu1Ba2Hg1: Tc = 97.0K (>= 95.0, qualifies)
- O4.160Cu1Ba2Hg1: no sample
- O4.162Cu1Ba2Hg1: Tc = 97.4K (>= 95.0, qualifies)
- O4.180Cu1Ba2Hg1: no sample
- O4.200Cu1Ba2Hg1: Tc = 94.2K (< 95.0, doesn't qualify)
- O4.220Cu1Ba2Hg1: Tc = 97.0K (>= 95.0, qualifies)

Materials with Hg and exactly 4 elements (Ca-Cu-Ba-Hg-O):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (< 95.0, doesn't qualify)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K (< 95.0, doesn't qualify)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K (>= 95.0, qualifies)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K (>= 95.0, qualifies)
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K (>= 95.0, qualifies)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (>= 95.0, qualifies)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (>= 95.0, qualifies)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K (>= 95.0, qualifies)
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K (>= 95.0, qualifies)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K (>= 95.0, qualifies)

Q1 qualifying materials (Tc >= 95.0):
- O4.081Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

threshold1 = 95.0

## Step C - Extrapolate (no measurement possible)
I need to look for other materials that have no samples available and induce a rule from measurable materials to predict high/low for unmeasurable ones.

From my searches, the unmeasurable materials were:
- O6.160Ca1Cu2Ba2Hg1 (from the original series)
- O6.270Ca1Cu2Ba2Hg1 (from the original series)
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

Wait, I also found this material: O6Cu1Ba2Hg1Tl1 (on page 9) - this has 5 elements (Cu, Ba, Hg, Tl, O), so it doesn't qualify for Step B.