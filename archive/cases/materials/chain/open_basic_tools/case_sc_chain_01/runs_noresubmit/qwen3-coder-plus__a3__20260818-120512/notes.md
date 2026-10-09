# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Definition: high Tc >= 90 K, low Tc < 90 K
- Tools: measure_tc() and search_db()

## Step A: Series Peak
Goal: Find the composition with maximum Tc in the homologous doping series (same elements, varying oxygen)

First, let me measure the seed material to establish a baseline:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K

I searched for materials in the homologous series (Ca, Cu, Ba, Hg with varying oxygen):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K

Maximum Tc (T*) = 127.0 K for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B: Filter Family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find materials containing Hg with exactly 4 elements that have Tc >= 95.0 K