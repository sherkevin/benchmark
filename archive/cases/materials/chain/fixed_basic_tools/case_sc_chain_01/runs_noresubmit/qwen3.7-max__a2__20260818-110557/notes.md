# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90 K, Low Tc: < 90 K

## Step A — Series Peak
Seed has elements: O, Ca, Cu, Ba, Hg (5 elements).
Homologous series = same element set, varying oxygen.
Series members from measurable.txt (lines 1-10):
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

## Step B — Filter Family B (Hg + exactly 4 elements)
Candidates from measurable.txt (lines 11-21):
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.047Cu1Ba2Hg1
8. O4.200Cu1Ba2Hg1
9. O4.101Cu1Ba2Hg1
10. O4.103Cu1Ba2Hg1
11. O4.085Cu1Ba2Hg1

These have elements: O, Cu, Ba, Hg (4 elements, contains Hg).

Budget: 10 (Step A) + 11 (Step B candidates) = 21 > 18. Need to be strategic - measure 8 of 11.

## Step C — Unmeasurable materials
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

## Measurements

### Step A - Series Members (10 measurements)
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (HIGH) **PEAK**
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (HIGH) **PEAK (tied)**
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K (HIGH)
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K (HIGH)

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)
**threshold1 = 127.0 - 32 = 95.0 K**

### Step B - Hg-containing 4-element materials (8 measurements)
1. O4.081Cu1Ba2Hg1: 98.0 K ✓
2. O4.162Cu1Ba2Hg1: 97.4 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K ✓
4. O4.220Cu1Ba2Hg1: 97.0 K ✓
5. O4.096Cu1Ba2Hg1: 95.6 K ✓
6. O4.097Cu1Ba2Hg1: 95.3 K ✓
7. O4.047Cu1Ba2Hg1: 95.0 K ✓
8. O4.200Cu1Ba2Hg1: 94.2 K ✗

Unmeasured (predicted >= 95 K based on pattern):
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1

**Q1 (qualifying):** All 10 materials except O4.200Cu1Ba2Hg1

### Step C - Rule Induction

**Observed Pattern:**
- All measured Hg-containing materials are HIGH (>= 90 K)
- 5-element (O,Ca,Cu,Ba,Hg): 92.0 - 127.0 K
- 4-element (O,Cu,Ba,Hg): 94.2 - 98.0 K

**Induced Rule:**
Materials are HIGH if they contain Hg (strong indicator of high-Tc superconductivity in cuprate families), OR if they contain specific high-Tc element combinations:
- Tl with Ca-Cu-Ba (thallium-based superconductors)
- Y-Ba-Cu-O (YBCO family)

Materials are LOW if they lack Hg AND have unusual element combinations or high complexity (6+ elements).

**Predictions for unmeasurable materials:**
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (contains Hg, 5 elements, similar to measured series)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (contains Hg, 5 elements, similar to measured series)
3. O4.180Cu1Ba2Hg1 → HIGH (contains Hg, 4 elements, similar to measured 4-element materials)
4. O8Ca0.850Cu2.200Ba2Tl1.800 → HIGH (Tl-based with Ca-Cu-Ba, known high-Tc family)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → LOW (contains Tl but unusual Sr-Pb combination, 6 elements - high complexity)
6. O8Cu3Y1Ba1.900La0.100 → HIGH (Y-Ba-Cu-O system, YBCO-like)
7. O8Cu4Y1Ba1.870La0.130 → HIGH (Y-Ba-Cu-O system, YBCO-like)

### Step D - Final Pick

Among predicted HIGH materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements **← FEWEST**
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O8Cu3Y1Ba1.900La0.100: 5 elements
- O8Cu4Y1Ba1.870La0.130: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, predicted HIGH based on Hg content and similarity to measured 4-element materials)
