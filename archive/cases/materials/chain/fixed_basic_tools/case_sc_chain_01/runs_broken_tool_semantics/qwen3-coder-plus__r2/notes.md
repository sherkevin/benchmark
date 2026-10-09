# Task Notes: Superconductor Candidate Analysis

## Initial Understanding
- Need to find the best unexplored superconductor candidate among materials in data/unmeasurable.txt
- "Best" = fewest elements among those with high Tc (Tc >= 90 K)
- Have a measurement budget of 45 calls to get_property
- Need to determine the qualifying set Q1 based on Hg-containing materials with exactly 4 elements
- Threshold1 = T* - 32, where T* is max Tc in the doping series of O6.080Ca1Cu2Ba2Hg1

## Step 1: Find the maximum Tc in the seed's doping series
First, I'll measure the Tc values for the homologous doping series of O6.080Ca1Cu2Ba2Hg1 to find T* (the maximum Tc).

Series compositions:
- O6.080Ca1Cu2Ba2Hg1
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1