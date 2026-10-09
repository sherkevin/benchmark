# Notes - Superconductor Tc Study

## Task Overview
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K

## Step A: Find homologous series (same elements, varying oxygen)
- Seed has elements: O, Ca, Cu, Ba, Hg
- Found 12 series members with varying oxygen content
- Measured all, found peak at Tc = 127.0 K

### Series Results:
| Composition | Oxygen | Tc (K) | Status |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | measurable |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | measurable |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | measurable |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | measurable |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | - | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | measurable |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | 127.0 | measurable (PEAK) |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | 127.0 | measurable (PEAK) |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | measurable |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | - | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | measurable |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | measurable |

**Peak Tc (T*): 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (first to reach max)

## Step B: 4-element Hg materials with Tc >= threshold1
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Found 14 four-element materials (O, Cu, Ba, Hg)
- Measured 10, found 4 unmeasurable

### 4-element Hg Results:
| Composition | Tc (K) | Status | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | - | UNMEASURABLE | - |
| O4.066Cu1Ba2Hg1 | - | UNMEASURABLE | - |
| O4.081Cu1Ba2Hg1 | 98.0 | measurable | ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | measurable | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | measurable | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | measurable | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | measurable | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | measurable | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | measurable | ✓ |
| O4.160Cu1Ba2Hg1 | - | UNMEASURABLE | - |
| O4.162Cu1Ba2Hg1 | 97.4 | measurable | ✓ |
| O4.180Cu1Ba2Hg1 | - | UNMEASURABLE | - |
| O4.200Cu1Ba2Hg1 | 94.2 | measurable | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | measurable | ✓ |

**Q1 (qualifying materials with Tc >= 95.0):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C: Rule induction and predictions

### Measured materials summary:
- All 5-element series (O-Ca-Cu-Ba-Hg): HIGH (Tc 92-127)
- All 4-element (O-Cu-Ba-Hg): HIGH (Tc 92-98)
- O6Cu1Ba2Hg1Tl1: LOW (Tc = 0.0) - has Tl, no Ca
- O10Ca2Cu3Ba2Tl2: HIGH (Tc = 118.97) - has Tl and Ca
- O6Cu1Cd0.250Ba2Tl1.750: LOW (Tc = 88.0) - has Tl, no Ca

### Induced Rule:
**"Contains Tl but not Ca → LOW; otherwise HIGH"**

This is a multi-factor rule (Tl presence AND Ca absence). Single-factor rules fail:
- "Has Tl → LOW" fails for O10Ca2Cu3Ba2Tl2 (HIGH)
- "No Ca → LOW" fails for all 4-element materials (HIGH)

### Unmeasurable materials and predictions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem): no Tl, has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 (5 elem): no Tl, has Ca → **HIGH**
3. O4.047Cu1Ba2Hg1 (4 elem): no Tl, no Ca → **HIGH**
4. O4.066Cu1Ba2Hg1 (4 elem): no Tl, no Ca → **HIGH**
5. O4.160Cu1Ba2Hg1 (4 elem): no Tl, no Ca → **HIGH**
6. O4.180Cu1Ba2Hg1 (4 elem): no Tl, no Ca → **HIGH**
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem): has Tl, has Ca → **HIGH**
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem): has Tl, has Ca → **HIGH**
9. O7Cu3Ba2La1 (4 elem): no Tl → **HIGH** (discovered late, assumed unmeasurable)
10. O7Cu3Y0.200Ba2La0.800 (5 elem): no Tl → **HIGH** (discovered late, assumed unmeasurable)
11. O8Cu3Y1Ba1.900La0.100 (5 elem): no Tl → **HIGH** (discovered late, assumed unmeasurable)
12. O8Cu4Y1Ba1.870La0.130 (5 elem): no Tl → **HIGH** (discovered late, assumed unmeasurable)

## Step D: Final pick
Among unmeasurable materials predicted HIGH, choose fewest elements, break ties by higher expected Tc.

Materials with 4 elements (fewest):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O7Cu3Ba2La1

Estimated Tc for O-Cu-Ba-Hg materials (interpolation from measured data):
- O4.160Cu1Ba2Hg1: ~97.2 (between 4.150→97.0 and 4.162→97.4)
- O4.180Cu1Ba2Hg1: ~95.8 (between 4.162→97.4 and 4.200→94.2)
- O4.047Cu1Ba2Hg1: ~95-98 (extrapolated, uncertain)
- O4.066Cu1Ba2Hg1: ~95-98 (extrapolated, uncertain)

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest estimated Tc ~97.2)

## Budget Tracking
- Used: 45 / 45 (exhausted)
- Searches: 16 pages (Hg: 7, Ca: 4, Tl: 2, Pb: 1, Sr: 1, Cd: 1, Cu: 3)
- Measurements: 29 attempts (21 successful, 8 unmeasurable)
