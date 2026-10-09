# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 45 units (shared between search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A: Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as O6.080Ca1Cu2Ba2Hg1.
The seed composition has elements: O, Ca, Cu, Ba, Hg.

Found materials in the series:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (MAX)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (MAX)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1) with T* = 127.0K

## Step B: Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0
Find materials containing Hg with exactly 4 elements where Tc >= 95.0.

Materials with Hg and exactly 4 elements:
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.081Cu1Ba2Hg1: Tc = 98.0K >= 95.0 ✓
- O4.085Cu1Ba2Hg1: Tc = 92.0K < 95.0
- O4.096Cu1Ba2Hg1: Tc = 95.6K >= 95.0 ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3K >= 95.0 ✓
- O4.101Cu1Ba2Hg1: Tc = 93.4K < 95.0
- O4.103Cu1Ba2Hg1: Tc = 92.5K < 95.0
- O4.150Cu1Ba2Hg1: Tc = 97.0K >= 95.0 ✓
- O4.160Cu1Ba2Hg1: No sample available
- O4.162Cu1Ba2Hg1: Tc = 97.4K >= 95.0 ✓
- O4.180Cu1Ba2Hg1: No sample available
- O4.200Cu1Ba2Hg1: Tc = 94.2K < 95.0
- O4.220Cu1Ba2Hg1: Tc = 97.0K >= 95.0 ✓

Set Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C: Extrapolate
Predict high/low for unmeasurable materials based on a rule.
Unmeasurable materials found so far:
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.160Cu1Ba2Hg1: No sample available
- O4.180Cu1Ba2Hg1: No sample available

Let me search more to find additional unmeasurable materials...

## Step D: Final Pick
Choose from unmeasurable high-predicted materials with fewest elements.

Let's continue...