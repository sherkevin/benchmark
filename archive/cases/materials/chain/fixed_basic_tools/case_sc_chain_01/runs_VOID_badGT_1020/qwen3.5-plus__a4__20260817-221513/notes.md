# Notes for Superconductor Tc Study

## Step A - Finding the peak Tc in the homologous series

The seed material is `O6.080Ca1Cu2Ba2Hg1`. The homologous series has the same element set (O, Ca, Cu, Ba, Hg) with varying oxygen.

Series members measured:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAX)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)

**threshold1 = T* - 32 = 95.0 K**

## Step B - Filter family B (contains Hg, exactly 4 elements, Tc >= 95.0)

4-element Hg materials (Cu1Ba2Hg1) measured:
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓ (Q1)
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓ (Q1)
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓ (Q1)
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓ (Q1)
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓ (Q1)
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓ (Q1)
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗
- O4.180Cu1Ba2Hg1: no sample
- O4.047Cu1Ba2Hg1: no sample
- O4.066Cu1Ba2Hg1: no sample
- O4.160Cu1Ba2Hg1: no sample

**Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]**

## Step C - Induced Rule

After analyzing measured data, the rule is:
**High Tc if (contains Ca) OR (contains Hg AND has exactly 4 elements). Otherwise Low.**

This rule correctly predicts all measured materials:
- Has Ca -> High (homologous series, O10Ca2Cu3Ba2Tl2)
- Has Hg + 4 elements + no Ca -> High (Cu1Ba2Hg1 series)
- Has Hg + 5 elements + no Ca -> Low (O6Cu1Ba2Hg1Tl1 with Tc=0)
- No Hg + No Ca -> Low (O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800)

## Step D - Final Pick

Among unmeasurable.txt materials predicted high, find those with fewest elements:
- 4-element materials predicted high: all Cu1Ba2Hg1 compositions (O4.xxxCu1Ba2Hg1)
- Among these, highest measured Tc is O4.081Cu1Ba2Hg1 at 98.0 K

**Final answer: O4.081Cu1Ba2Hg1**

## Budget used: 25/34 measurements

