# Notes

## Task Overview
- Budget: 18 units (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Step A: Find homologous series peak
- Seed: O6.080Ca1Cu2Ba2Hg1 (elements: O, Ca, Cu, Ba, Hg - 5 elements)
- Series: same element set, varying oxygen
- Need to find all members and measure Tc

## Budget tracking
- Used: 18/18 (9 search pages + 9 successful measurements; 2 failed measurements were free)
- Remaining: 0

## All Hg-containing materials found (27 total):

### 4-element (O, Cu, Ba, Hg) - 14 materials:
1. O4.047Cu1Ba2Hg1 - unmeasured
2. O4.066Cu1Ba2Hg1 - unmeasured
3. O4.081Cu1Ba2Hg1 - unmeasured
4. O4.085Cu1Ba2Hg1 - unmeasured
5. O4.096Cu1Ba2Hg1 - unmeasured
6. O4.097Cu1Ba2Hg1 - unmeasured
7. O4.101Cu1Ba2Hg1 - MEASURED: Tc=93.4 (high)
8. O4.103Cu1Ba2Hg1 - unmeasured
9. O4.150Cu1Ba2Hg1 - MEASURED: Tc=97.0 (high)
10. O4.160Cu1Ba2Hg1 - unmeasured
11. O4.162Cu1Ba2Hg1 - unmeasured
12. O4.180Cu1Ba2Hg1 - unmeasured
13. O4.200Cu1Ba2Hg1 - unmeasured
14. O4.220Cu1Ba2Hg1 - unmeasured

### 5-element (O, Ca, Cu, Ba, Hg) - homologous series - 12 materials:
15. O6.080Ca1Cu2Ba2Hg1 - MEASURED: Tc=92.0 (high) [seed]
16. O6.100Ca1Cu2Ba2Hg1 - MEASURED: Tc=92.5 (high)
17. O6.130Ca1Cu2Ba2Hg1 - unmeasured
18. O6.140Ca1Cu2Ba2Hg1 - unmeasured
19. O6.160Ca1Cu2Ba2Hg1 - UNMEASURABLE (no sample)
20. O6.190Ca1Cu2Ba2Hg1 - MEASURED: Tc=122.0 (high)
21. O6.210Ca1Cu2Ba2Hg1 - unmeasured
22. O6.220Ca1Cu2Ba2Hg1 - MEASURED: Tc=127.0 (high) ← PEAK
23. O6.240Ca1Cu2Ba2Hg1 - MEASURED: Tc=123.0 (high)
24. O6.270Ca1Cu2Ba2Hg1 - UNMEASURABLE (no sample)
25. O6.300Ca1Cu2Ba2Hg1 - unmeasured
26. O6.310Ca1Cu2Ba2Hg1 - MEASURED: Tc=98.0 (high)

### 5-element (O, Cu, Ba, Hg, Tl) - 1 material:
27. O6Cu1Ba2Hg1Tl1 - MEASURED: Tc=0.0 (low)

## Analysis

### Step A: Series peak
- T* = 127.0 K at O6.220Ca1Cu2Ba2Hg1
- Series shows parabolic dome: 92.0 → 92.5 → 122.0 → 127.0 → 123.0 → 98.0

### Step B: threshold1 = 127.0 - 32 = 95.0
- 4-element Hg materials with Tc >= 95.0:
  - O4.150Cu1Ba2Hg1: Tc=97.0 ✓ (confirmed)
  - O4.101Cu1Ba2Hg1: Tc=93.4 ✗

### Step C: Unmeasurable materials
- O6.160Ca1Cu2Ba2Hg1: predicted HIGH (Ca-containing series member, parabolic fit ~104 K)
- O6.270Ca1Cu2Ba2Hg1: predicted HIGH (Ca-containing series member, parabolic fit ~106 K)

Rule: Materials containing Ca with 5 elements (O,Ca,Cu,Ba,Hg) are high; materials with 4 elements (O,Cu,Ba,Hg) are high; materials containing Tl without Ca are low. Multi-factor: high if (contains Ca) OR (has exactly 4 elements O,Cu,Ba,Hg); low if (contains Tl AND lacks Ca).

### Step D: Final pick
- Both unmeasurable high-predicted materials have 5 elements
- O6.270 has higher expected Tc (~106 K vs ~104 K)
- Final pick: O6.270Ca1Cu2Ba2Hg1
