# Superconductor Tc Study Notes

## Step A: Series Peak Analysis

Starting with the seed material: O6.080Ca1Cu2Ba2Hg1

I need to measure all compositions in the homologous series (same element set, varying oxygen) from measurable.txt to find the one with maximum Tc.

Results for Ca1Cu2Ba2Hg1 series (varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Peak composition: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both have Tc = 127.0 K)
Maximum Tc (T*) = 127.0 K

## Step B: Filter Family B (contains Hg and has exactly 4 elements)

threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg and having exactly 4 elements from measurable.txt:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥ 95.0 K) ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K (≥ 95.0 K) ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0 K) ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0 K) ✓
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K (≥ 95.0 K) ✓
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K (≥ 95.0 K) ✓
7. O4.047Cu1Ba2Hg1: Tc = 95.0 K (≥ 95.0 K) ✓
8. O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0 K) ✗

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C & D: Analysis of Unmeasurable Materials

Unfortunately, I've used all 18 measurements, so I cannot measure the remaining materials in measurable.txt that don't belong to the main series or family B. However, I can still analyze the unmeasurable materials based on patterns observed.

Remaining materials from measurable.txt:
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

From the data I have collected, I can now try to determine a rule for predicting high/low Tc for unmeasurable materials.

Looking at the pattern from measurable materials:
- Materials with higher oxygen content around 6.21-6.22 tend to have high Tc
- Materials with Hg and Cu, Ba (4 elements) with lower oxygen content around 4.08-4.22 also have relatively high Tc values
- The highest Tc materials seem to have specific oxygen stoichiometries

For unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Similar to high Tc series, likely high
2. O6.270Ca1Cu2Ba2Hg1 - Similar to high Tc series, likely high  
3. O4.180Cu1Ba2Hg1 - Similar to high Tc series, likely high
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Complex with Tl, likely high due to high oxygen content
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Sr and Pb instead of Ba, different structure
6. O8Cu3Y1Ba1.900La0.100 - Contains Y and La instead of Ca, different structure
7. O8Cu4Y1Ba1.870La0.130 - Contains Y and La instead of Ca, different structure

Based on my observations, the rule seems to involve:
- Oxygen stoichiometry being in optimal ranges
- Presence of certain cations (like Hg, Ba, Cu, Ca)
- Specific ratios between different elements