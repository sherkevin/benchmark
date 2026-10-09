# Notes

## Task Overview
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Budget Tracking
- Used: 8 (1 measurement + 7 search pages for Hg)
- Remaining: 37

## All Hg-containing materials found (27 total):

### 4-element materials (O, Cu, Ba, Hg) - "Family B":
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

### 5-element homologous series (O, Ca, Cu, Ba, Hg) - Step A:
1. O6.080Ca1Cu2Ba2Hg1 (SEED, Tc = 92.0)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

### Other (5 elements):
- O6Cu1Ba2Hg1Tl1 (O, Cu, Ba, Hg, Tl)

## Step A Results (Homologous Series)
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

**T* = 127.0 K, Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B Results
threshold1 = 127.0 - 32 = 95.0 K

4-element Hg materials (O, Cu, Ba, Hg) measurements:
- O4.047Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.066Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.081Cu1Ba2Hg1: 98.0 K ✓ (>= 95.0)
- O4.085Cu1Ba2Hg1: 92.0 K ✗
- O4.096Cu1Ba2Hg1: 95.6 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K ✓
- O4.101Cu1Ba2Hg1: 93.4 K ✗
- O4.103Cu1Ba2Hg1: 92.5 K ✗
- O4.150Cu1Ba2Hg1: 97.0 K ✓
- O4.160Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.162Cu1Ba2Hg1: 97.4 K ✓
- O4.180Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.200Cu1Ba2Hg1: 94.2 K ✗
- O4.220Cu1Ba2Hg1: 97.0 K ✓

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

## Step C Results

### Other materials measured:
- O6Cu1Ba2Hg1Tl1: 0.0 K (LOW) - 5 elements
- O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH) - 5 elements
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (LOW) - 5 elements
- O7Cu3Ba2La1: 89.6 K (LOW) - 4 elements
- O7Cu3Y0.200Ba2La0.800: 81.0 K (LOW) - 5 elements

### Unmeasurable materials found (10 total):
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg)
3. O4.047Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
4. O4.066Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
5. O4.160Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
6. O4.180Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 - 6 elements (O, Ca, Cu, Sr, Tl, Pb)
8. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements (O, Ca, Cu, Ba, Tl)
9. O8Cu3Y1Ba1.900La0.100 - 4 elements (O, Cu, Y, Ba, La)
10. O8Cu4Y1Ba1.870La0.130 - 4 elements (O, Cu, Y, Ba, La)

### Rule induced:
**HIGH if: (contains Hg AND NOT Tl) OR contains Ca**
**LOW if: (no Hg OR has Tl) AND no Ca**

This multi-factor rule is consistent with all 25 measured materials:
- All Hg-only materials (no Tl): HIGH ✓
- Materials with Ca: HIGH ✓
- Materials without Hg and without Ca: LOW ✓
- O6Cu1Ba2Hg1Tl1 (has both Hg and Tl, no Ca): LOW (0.0 K) ✓

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Ca and Hg, no Tl → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca and Hg, no Tl → **HIGH**
3. O4.047Cu1Ba2Hg1: has Hg, no Tl, no Ca → **HIGH**
4. O4.066Cu1Ba2Hg1: has Hg, no Tl, no Ca → **HIGH**
5. O4.160Cu1Ba2Hg1: has Hg, no Tl, no Ca → **HIGH**
6. O4.180Cu1Ba2Hg1: has Hg, no Tl, no Ca → **HIGH**
7. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH**
8. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH**
9. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → **LOW**
10. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → **LOW**

## Step D Results

Among unmeasurable predicted HIGH, find fewest elements:
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Four materials tied at 4 elements. Break tie by higher expected Tc.

Looking at measurable 4-element Hg materials sorted by oxygen:
- O4.081 → 98.0 K (peak)
- O4.085 → 92.0 K
- O4.096 → 95.6 K
- O4.097 → 95.3 K
- O4.101 → 93.4 K
- O4.103 → 92.5 K
- O4.150 → 97.0 K
- O4.162 → 97.4 K (second peak)
- O4.200 → 94.2 K
- O4.220 → 97.0 K

Unmeasurable ones:
- O4.047: very low O, extrapolate → ~92-94 K
- O4.066: low O, extrapolate → ~95-96 K
- O4.160: between O4.150 (97.0) and O4.162 (97.4) → ~97.2 K (highest expected)
- O4.180: between O4.162 (97.4) and O4.200 (94.2) → ~95.8 K

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ~97.2 K)

## Budget Summary
- Total used: 44 out of 45
- Measurements: 25 successful, 10 failed (no sample, no cost)
- Searches: 9 pages (7 Hg + 2 Cu... wait, let me recount)
  - Hg: 7 pages
  - Cu: 9 pages
  - Ba: 1 page
  - Tl: 2 pages
  Total: 19 search pages = 19 budget
- Successful measurements: 25 = 25 budget
- Total: 19 + 25 = 44 ✓
