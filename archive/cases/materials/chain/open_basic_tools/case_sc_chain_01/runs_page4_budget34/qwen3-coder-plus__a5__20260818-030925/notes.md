# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units total (for both search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A - Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as O6.080Ca1Cu2Ba2Hg1.
The seed has elements: O, Ca, Cu, Ba, Hg with formula O6.080Ca1Cu2Ba2Hg1

Let me start by measuring the Tc of the seed material to establish a baseline.
- O6.080Ca1Cu2Ba2Hg1 has Tc = 92.0K

Now I need to find other materials in the same homologous series (same elements, varying oxygen).
The elements in the seed are: O, Ca, Cu, Ba, Hg

## Step A - Series Peak Results
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Sample not available
- O6.160Ca1Cu2Ba2Hg1: Tc = ? (not measured, sample not available)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: Sample not available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Maximum Tc (T*) = 127.0K
Peak composition = O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 (both have same Tc)

## Step B - Filter Family B
Family B: Contains Hg and has exactly 4 elements
Threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Need to find materials that contain Hg and have exactly 4 elements, then measure those with Tc >= 95.0K

Materials with Hg and exactly 4 elements (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available  
- O4.081Cu1Ba2Hg1: Tc = 98.0K >= 95.0K ✓ (Q1)
- O4.085Cu1Ba2Hg1: Tc = 92.0K < 95.0K
- O4.096Cu1Ba2Hg1: Tc = 95.6K >= 95.0K ✓ (Q1)
- O4.097Cu1Ba2Hg1: Tc = 95.3K >= 95.0K ✓ (Q1)
- O4.101Cu1Ba2Hg1: Tc = 93.4K < 95.0K
- O4.103Cu1Ba2Hg1: Tc = 92.5K < 95.0K
- O4.150Cu1Ba2Hg1: Tc = 97.0K >= 95.0K ✓ (Q1)
- O4.160Cu1Ba2Hg1: No sample available
- O4.162Cu1Ba2Hg1: Tc = 97.4K >= 95.0K ✓ (Q1)
- O4.180Cu1Ba2Hg1: No sample available
- O4.200Cu1Ba2Hg1: Tc = 94.2K < 95.0K
- O4.220Cu1Ba2Hg1: Tc = 97.0K >= 95.0K ✓ (Q1)

So Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C - Extrapolate
Some materials could not be measured because they're not synthesized yet:
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.160Cu1Ba2Hg1: No sample available
- O4.180Cu1Ba2Hg1: No sample available

Plus I need to find more unmeasurable materials in the database by searching further.