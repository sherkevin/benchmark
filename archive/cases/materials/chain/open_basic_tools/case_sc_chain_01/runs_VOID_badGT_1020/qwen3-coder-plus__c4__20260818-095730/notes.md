# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: 34 units total
- Tools: measure_tc() and search_db()

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find series members and measure their Tc to identify the maximum Tc (T*).

First, let me measure the seed material to establish a baseline:

- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K

## Step A - Series Peak Results
I measured the Tc of various materials in the homologous series (OxCa1Cu2Ba2Hg1):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Tc = no sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Tc = no sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

So the peak composition is either O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 with T* = 127.0 K.

## Step B - Filter Family B
Family B: Contains Hg and has exactly 4 elements.
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
I need to find materials that contain Hg and have exactly 4 elements, then report ALL whose Tc >= 95.0 K.

I've examined all the Hg-containing materials and identified those with exactly 4 elements:
- O4.047Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.066Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.081Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.085Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.096Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.097Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.101Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.103Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.150Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.160Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.162Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.180Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.200Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O4.220Cu1Ba2Hg1: 3 elements (Cu, Ba, Hg) - Doesn't qualify
- O6.080Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 92.0 K (< 95.0 K)
- O6.100Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 92.5 K (< 95.0 K)
- O6.130Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 102.0 K (>= 95.0 K) ✓
- O6.140Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 105.0 K (>= 95.0 K) ✓
- O6.160Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, no sample available
- O6.190Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 122.0 K (>= 95.0 K) ✓
- O6.210Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 127.0 K (>= 95.0 K) ✓
- O6.220Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 127.0 K (>= 95.0 K) ✓
- O6.240Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 123.0 K (>= 95.0 K) ✓
- O6.270Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, no sample available
- O6.300Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 100.0 K (>= 95.0 K) ✓
- O6.310Ca1Cu2Ba2Hg1: 4 elements (Ca, Cu, Ba, Hg) - Qualifies, Tc = 98.0 K (>= 95.0 K) ✓
- O6Cu1Ba2Hg1Tl1: 4 elements (Cu, Ba, Hg, Tl) - Qualifies, Tc = 0.0 K (< 95.0 K)

So Q1 (materials containing Hg with exactly 4 elements and Tc >= 95.0 K) includes:
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

## Step C - Extrapolate
I need to find unmeasurable materials (those that return "no sample exists") and predict whether they are high or low Tc based on measurable materials.
From my searches, I found:
- O6.160Ca1Cu2Ba2Hg1: no sample available
- O6.270Ca1Cu2Ba2Hg1: no sample available

I also need to search for additional unmeasurable materials that I haven't found yet. Let me search more broadly: