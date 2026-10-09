# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Both search_db and measure_tc cost 1 unit each
- High material: Tc >= 90 K
- Low material: Tc < 90 K

## Step A: Find the series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find members of this series and measure their Tc to identify the maximum Tc (T*).

I searched for materials with similar composition to find the homologous series with varying oxygen content:
- Found 12 materials in the series: O6.080Ca1Cu2Ba2Hg1 through O6.310Ca1Cu2Ba2Hg1

I measured 10 out of 12 materials in the series (couldn't measure 2 due to sample availability):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

So T* = 127.0 K (for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

## Step B: Filter family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0

I searched for materials containing Hg:
- Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1

All these materials have exactly 4 elements (O, Cu, Ba, Hg), so they qualify.
However, I ran out of budget after searching (used all 18 units) and could not measure any of these materials.
Therefore, I cannot determine which ones have Tc >= 95.0 K.

## Step C: Extrapolate (no measurement possible)
From the measurable materials in the series, I can see that Tc varies with oxygen content.
Unmeasurable materials I found:
- O6.160Ca1Cu2Ba2Hg1: No sample
- O6.270Ca1Cu2Ba2Hg1: No sample

## Step D: Final pick
Since I couldn't measure the 4-element Hg materials, I can't complete Step B properly, which affects Step D.

## Budget Usage
- Used 5 units for searching the homologous series
- Used 10 units for measuring 10 materials in the series
- Used 3 units for searching Hg-containing materials
- Total: 18 units (all budget used)