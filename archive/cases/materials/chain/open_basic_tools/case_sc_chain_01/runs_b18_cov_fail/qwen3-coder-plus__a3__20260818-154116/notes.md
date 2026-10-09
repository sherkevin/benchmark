# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 18 units (shared between search_db and measure_tc)
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A: Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find series members and measure to identify MAXIMUM Tc (= T*).

First, I'll search for materials similar to the seed (containing same elements but varying oxygen).

Results from measuring the series O6.xx0Ca1Cu2Ba2Hg1:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K

Maximum Tc (T*) = 127.0K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have the same Tc)

## Step B: Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= threshold1.