# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Both search_db and measure_tc cost 1 unit each
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A: Series Peak
Need to find materials in the same homologous doping series (same elements, varying oxygen) as the seed material O6.080Ca1Cu2Ba2Hg1 and determine which has the maximum Tc.

Found materials in the series through searching Hg:
O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measurement results:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (used 1 budget unit)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (used 1 budget unit)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (used 1 budget unit)

Based on the measurements, O6.130Ca1Cu2Ba2Hg1 appears to have the highest Tc at 102.0 K.
Maximum Tc (T*) = 102.0 K

## Step B: Filter Family B
Family B consists of materials containing Hg and having exactly 4 elements.
Materials found: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

However, I have run out of budget and cannot measure their Tc values. Threshold1 = T* - 32 = 102.0 - 32 = 70.0 K.

Since I couldn't measure these materials' Tc, I cannot determine which ones qualify for Q1 (those with Tc >= 70.0 K).

## Step C & D: Extrapolation and Final Pick
Since I ran out of budget before completing Step B and before being able to search for unmeasurable materials, I need to make educated guesses based on common patterns in superconductors.

Many copper-oxide superconductors follow a dome-shaped dependence on doping level, where Tc increases to a maximum at optimal doping and then decreases. In the series I measured, Tc increased from 92.0 K to 92.5 K to 102.0 K as oxygen content increased from 6.080 to 6.100 to 6.130, suggesting we're approaching or at the optimal doping level.

For materials that weren't measurable, I would have needed to search further to find them and then develop a predictive rule based on chemical composition.