# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Definition: high Tc >= 90 K, low Tc < 90 K
- Tools: measure_tc (costs 1 unit), search_db (costs 1 unit per page)

## Step A - Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as O6.080Ca1Cu2Ba2Hg1
The elements are: O, Ca, Cu, Ba, Hg (with stoichiometry O6.080Ca1Cu2Ba2Hg1)

Let me search for materials in this series by looking for materials with these elements but different oxygen content.

Results from series measurement:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Tc = null (no sample available)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX Tc)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX Tc)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Tc = null (no sample available)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1) with T* = 127.0 K

## Step B - Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

I searched for materials containing Hg and found the following with exactly 4 elements (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

Unfortunately, I have used all my budget (18 units) and cannot measure these materials. I had used 14 units on series measurements and 4 units on Hg searches.

## Step C & D - Prediction and Final Pick
I need to work with the data I have so far.
From the series measurements, I have:
Measurable materials with known Tc:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (low)
- O6.085Cu1Ba2Hg1: Tc = ? (not measured)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (low)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (high)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (high)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (high)

Unmeasurable materials (no sample available):
- O6.160Ca1Cu2Ba2Hg1: (unmeasurable)
- O6.270Ca1Cu2Ba2Hg1: (unmeasurable)

Based on the oxygen content pattern in the measurable series, I can observe:
- Lower oxygen content (6.080, 6.100) gives lower Tc (92.0, 92.5 K)
- Medium oxygen content (6.130, 6.140) gives medium-high Tc (102.0, 105.0 K)
- Higher oxygen content (6.190-6.240) gives very high Tc (122.0-127.0 K)
- Very high oxygen content (6.270-6.310) shows decrease (not measured for 6.270, then 100.0, 98.0 K)

For the unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: This falls between O6.140 (Tc=105.0K) and O6.190 (Tc=122.0K), so likely high Tc ~115K (prediction: high)
- O6.270Ca1Cu2Ba2Hg1: This falls between O6.240 (Tc=123.0K) and O6.300 (Tc=100.0K), suggesting it might be in the decreasing part of the curve, possibly around 110-120K (prediction: high)

For the 4-element Hg materials, since I couldn't measure them but they have lower oxygen content (around 4.x vs 6.x in the series above), they might have different properties. Based on the general trend that higher oxygen content tends to increase Tc in cuprates, these might have lower Tc. However, since I don't have specific data for these compounds, I'll need to make educated guesses based on the structural similarity.

Rule induction: Looking at the measurable materials in the 5-element series, there appears to be a correlation between oxygen content and Tc, with an optimal range around 6.190-6.240 giving maximum Tc, and then decreasing again at higher oxygen content.