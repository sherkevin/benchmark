# Notes on Superconductor Tc Study

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: 34 measure_tc calls

## Step A — Series Peak
The seed belongs to a homologous doping series with same element set (O, Ca, Cu, Ba, Hg), varying oxygen.

Looking at measurable.txt, the series members appear to be:
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

Also from the top section of measurable.txt:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

Need to measure all of these to find the maximum Tc (T*).

## Measurements to make

## Step A Results — Series Peak
Measured all series members (same element set as seed: O, Ca, Cu, Ba, Hg):

| Composition | Tc (K) |
|-------------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition: **O6.210Ca1Cu2Ba2Hg1**

Budget used: 10 measurements

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Need to find materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= 95.0

From measurable.txt, materials with Hg and exactly 4 elements:
- O4.180Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.081Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.162Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.150Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.220Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.096Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.097Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.047Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.200Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.101Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.103Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)
- O4.085Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg = 4)

Need to measure these to find which have Tc >= 95.0

## Step B Results
threshold1 = 95.0

| Composition | Tc (K) | >= 95.0? |
|-------------|--------|----------|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.180Cu1Ba2Hg1 | no sample | - |

**Q1** (qualifying materials): O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1

Budget used: 21 measurements. 13 remaining.

## Step C — Extrapolate to Unmeasurable Materials

Unmeasurable compositions (need to predict high/low):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

### Induced Rule
After analyzing all measured data:

**High Tc materials:**
- All Ca-Cu-Ba-Hg series (5 elem): 92-127 K - Has Hg, no Tl
- All Cu-Ba-Hg 4-element: 92-98 K - Has Hg, no Tl
- O10Ca2Cu3Ba2Tl2: 118.97 K - Has Ca AND Tl

**Low Tc materials:**
- O6Cu1Ba2Hg1Tl1: 0.0 K - Has both Hg AND Tl
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K - Has Tl but no Ca
- O7Cu3Ba2La1: 89.6 K - No Hg, no Ca
- O7Cu3Y0.200Ba2La0.800: 81.0 K - No Hg, no Ca

**Rule: High Tc if (contains Hg AND does NOT contain Tl) OR (contains Ca AND contains Tl)**

Equivalently: A material is HIGH if it has Hg without Tl, OR it has both Ca and Tl.

### Predictions for Unmeasurable
1. O6.160Ca1Cu2Ba2Hg1: Has Hg, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Has Hg, no Tl → HIGH
3. O4.180Cu1Ba2Hg1: Has Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Has Ca AND Tl → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Ca AND Tl → HIGH
6. O8Cu3Y1Ba1.900La0.100: No Hg, no Ca → LOW
7. O8Cu4Y1Ba1.870La0.130: No Hg, no Ca → LOW

## Step D — Final Pick
Among HIGH predictions, find one with FEWEST elements:

- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, the fewest among HIGH predictions)

## Summary for answer.json
- peak: "O6.210Ca1Cu2Ba2Hg1"
- peak_tc: 127.0
- threshold: 95.0
- qualifying: ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]
- predictions: all 7 unmeasurable with high/low
- final: "O4.180Cu1Ba2Hg1"
- rule: "High Tc if (contains Hg AND NOT Tl) OR (contains Ca AND Tl)"

Budget used: 26 measurements (8 remaining)
