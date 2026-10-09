# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (for both search_db and measure_tc)
- Definition: high Tc >= 90K, low Tc < 90K

## Step A - Series Peak (Homologous doping series)
- Need to find materials with same elements as seed (O, Ca, Cu, Ba, Hg) but varying oxygen
- Find the composition with maximum Tc (T*)

Found 12 materials in the series:
1. O6.080Ca1Cu2Ba2Hg1 - Tc: 92.0K
2. O6.100Ca1Cu2Ba2Hg1 - Tc: 92.5K
3. O6.130Ca1Cu2Ba2Hg1 - Tc: 102.0K
4. O6.140Ca1Cu2Ba2Hg1 - Tc: 105.0K
5. O6.160Ca1Cu2Ba2Hg1 - Tc: No sample available
6. O6.190Ca1Cu2Ba2Hg1 - Tc: 122.0K
7. O6.210Ca1Cu2Ba2Hg1 - Tc: 127.0K
8. O6.220Ca1Cu2Ba2Hg1 - Tc: 127.0K
9. O6.240Ca1Cu2Ba2Hg1 - Tc: 123.0K
10. O6.270Ca1Cu2Ba2Hg1 - Tc: No sample available
11. O6.300Ca1Cu2Ba2Hg1 - Tc: 100.0K
12. O6.310Ca1Cu2Ba2Hg1 - Tc: 98.0K

Step A Results:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both have Tc = 127.0K)
- T* = 127.0K

Step B Results:
- threshold1 = T* - 32 = 127.0 - 32 = 95.0K
- Materials containing Hg with exactly 4 elements that have Tc >= 95.0K:
  - O4.081Cu1Ba2Hg1 (Tc: 98.0K)
  - O4.096Cu1Ba2Hg1 (Tc: 95.6K)
  - O4.097Cu1Ba2Hg1 (Tc: 95.3K)
  - O4.150Cu1Ba2Hg1 (Tc: 97.0K)
  - O4.162Cu1Ba2Hg1 (Tc: 97.4K)
  - O4.220Cu1Ba2Hg1 (Tc: 97.0K)
- Q1 = ["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

Step C & D Results:
- Unmeasurable materials found:
  1. O6.160Ca1Cu2Ba2Hg1 (5 elements) - No sample available
  2. O6.270Ca1Cu2Ba2Hg1 (5 elements) - No sample available
  3. O4.047Cu1Ba2Hg1 (4 elements) - No sample available
  4. O4.066Cu1Ba2Hg1 (4 elements) - No sample available
  5. O4.160Cu1Ba2Hg1 (4 elements) - No sample available
  6. O4.180Cu1Ba2Hg1 (4 elements) - No sample available

Rule: Based on oxygen content correlation with Tc - materials with intermediate to high oxygen content in their respective series tend to have higher Tc values, following a bell-shaped curve pattern where there's an optimal oxygen doping level for maximum Tc.

Predictions:
- O6.160Ca1Cu2Ba2Hg1 → High (interpolated between neighboring high Tc values)
- O6.270Ca1Cu2Ba2Hg1 → Low (beyond the peak in the 5-element series)
- O4.047Cu1Ba2Hg1 → Low (very low oxygen content)
- O4.066Cu1Ba2Hg1 → Low (low oxygen content)
- O4.160Cu1Ba2Hg1 → High (in the high Tc range of 4-element series)
- O4.180Cu1Ba2Hg1 → High (in the high Tc range of 4-element series)

Step D: Among unmeasurable materials predicted as high, choosing the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O4.160Cu1Ba2Hg1 (4 elements) - fewer elements
- O4.180Cu1Ba2Hg1 (4 elements) - fewer elements

Between O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1 (both with 4 elements), O4.160Cu1Ba2Hg1 is closer to O4.162 (which had Tc=97.4K), suggesting it might have a higher Tc than O4.180Cu1Ba2Hg1.

Final pick for Step D: O4.160Cu1Ba2Hg1