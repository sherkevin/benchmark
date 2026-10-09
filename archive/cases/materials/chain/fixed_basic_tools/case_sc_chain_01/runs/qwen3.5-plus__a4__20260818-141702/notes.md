# Notes for Superconductor Tc Study - Final

## Step A: Series Peak
Measured the homologous series (same element set as seed O6.080Ca1Cu2Ba2Hg1, varying oxygen).

**Results:**
- Ca1Cu2Ba2Hg1 series (5 elements):
  - O6.080: 92.0 K, O6.100: 92.5 K, O6.130: 102.0 K, O6.140: 105.0 K
  - O6.190: 122.0 K, O6.210: 127.0 K (PEAK), O6.220: 127.0 K
  - O6.240: 123.0 K, O6.300: 100.0 K, O6.310: 98.0 K

- Cu1Ba2Hg1 series (4 elements):
  - O4.081: 98.0 K, O4.162: 97.4 K, O4.150: 97.0 K, O4.220: 97.0 K
  - O4.096: 95.6 K, O4.097: 95.3 K, O4.047: 95.0 K, O4.200: 94.2 K

**Peak (T*):** 127.0 K at O6.210Ca1Cu2Ba2Hg1

## Step B: Filter Family B (Q1)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0
- Criteria: Contains Hg, exactly 4 elements, Tc >= 95.0

**Q1 (7 materials):**
- O4.047Cu1Ba2Hg1 (95.0), O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0), O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0)

## Step C: Induced Rule
All measured materials are HIGH (>= 90 K). They all contain Cu, Ba, and Hg.

**Rule:** Material is HIGH if it contains Cu, Ba, and at least one of {Hg, Tl, Y}. Otherwise LOW.

This rule accounts for:
- Hg-based cuprates (measured, all HIGH)
- Tl-based cuprates (known high-Tc superconductors)
- Y-based cuprates like YBCO (Tc ~92 K, HIGH)
- La-only materials like LSCO (Tc ~40 K, LOW)

**Predictions:**
- 25 HIGH: All materials except O7Cu3Ba2La1
- 1 LOW: O7Cu3Ba2La1 (has La but no Hg/Tl/Y)

## Step D: Final Pick
Among predicted HIGH unmeasurable materials, find fewest elements:
- 4-element HIGH: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1
- Tie-break by expected Tc: O4.085 has highest (~96-97 K based on trend)

**Final Answer:** O4.085Cu1Ba2Hg1

## Budget Used: 18/18 measurements
