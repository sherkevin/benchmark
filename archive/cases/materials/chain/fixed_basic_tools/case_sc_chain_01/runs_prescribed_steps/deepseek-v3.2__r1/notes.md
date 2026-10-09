# Superconductor Tc Study Notes

## Step A: Find Series Peak T*

### Initial Plan
- Measurement budget: 45 `get_property` calls
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Need to find composition with MAXIMUM Tc among materials in `data/measurable.txt`

### Observations
1. The measurable.txt contains 30 materials
2. First 12 materials appear to be a homologous doping series with varying oxygen content: O6.080 to O6.310
3. Next 12 materials (lines 13-24) appear to be another series with different stoichiometry
4. Last 6 materials are diverse compositions

### Measurement Strategy
Given limited budget (45 measurements for all steps), I need to:
1. First measure the homologous series to find T* 
2. Then measure materials for Step B
3. Save measurements for understanding the rule in Step C

Let me start by measuring the homologous series (lines5581-12).

### Homologous Series Measurements (O6.xxxCa1Cu2Ba2Hg1)

1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: NOT MEASURABLE (null)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: NOT MEASURABLE (null)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

### Observations from Homologous Series
- Highest Tc measured so far: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
- Two materials in this series are unmeasurable: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- Tc appears to increase with oxygen content up to a point, then decreases

### O4.xxxCu1Ba2Hg1 Series Measurements
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K
5. O4.180Cu1Ba2Hg1: NOT MEASURABLE (null)
6. O4.047Cu1Ba2Hg1: NOT MEASURABLE (null)

All measured Tc values in this series are around 97-98 K (high by definition but lower than first series).

### Measurement Status
Used: пода14 measurements
Remaining: 31 measurements

### Diverse Compositions Measurements
1. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (low)
2. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (high)
3. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (low)
4. O7Cu3Ba2La1: Tc = 89.6 K (low)
5. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (low)

### Current Maximum Tc
Highest Tc measured: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

### Step A Result
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K

### Step B: Filter Family B
- threshold1 = T* - 32 = 127 - 32 = 95 K
- Materials must contain Hg and have exactly 4 elements
- Only O4.xxxCu1Ba2Hg1 series qualifies (O, Cu, Ba, Hg = 4 elements)

#### Measurements for O4 series:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (>= 95)
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓ (>= 95)
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓ (>= 95)
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ (>= 95)
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓ (>= 95)
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓ (>= 95)
7. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗ (< 95)
8. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✗ (< 95)
9. O4.103Cu1Ba2Hg1: Tc = 92.5 K ✗ (< 95)
10. O4.085Cu1Ba2Hg1: Tc = 92.0 K ✗ (< 95)
11. O4.180Cu1Ba2Hg1: NOT MEASURABLE (in unmeasurable.txt)
12. O4.047Cu1Ba2Hg1: NOT MEASURABLE (in unmeasurable.txt)

#### Qualifying Set Q1:
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1

### Measurement Status: 25 measurements
Remaining: 20 measurements

### Step C: Induce Rule for High/Low Tc

#### Current Dataset
**High Tc (≥90 K):** 17 materials
- O6.xxxCa1Cu2Ba2Hg1 series (9 measurable ones): 92-127 K
- O4.xxxCu1Ba2Hg1 series (10 measurable ones): 92-98 K  
- O10Ca2Cu3Ba2Tl2: 118.97 K

**Low Tc (<90 K):** 4 materials
1. O6Cu1Ba2Hg1Tl1: 0.0 K (contains Hg AND Tl, no Ca)
2. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (contains Tl, no Ca)
3. O7Cu3Ba2La1: 89.6 K (contains La, no Ca)
4. O7Cu3Y0.200Ba2La0.800: 81.0 K (contains La, no Ca)

#### Pattern Analysis
1. All Hg-containing materials without Tl are high Tc
2. Tl-containing materials without Ca are low Tc (2 examples)
3. Tl-containing material with Ca is high Tc (O10Ca2Cu3Ba2Tl2)
4. La-containing materials without Ca are low Tc (2 examples)
5. No examples of La-containing materials with Ca

#### Hypothesized Rule
"Materials containing Tl or La are low Tc unless they also contain Ca."

#### Testing the Rule
- O6Cu1Ba2Hg1Tl1: Tl, no Ca → low ✓
- O6Cu1Cd0.250Ba2Tl1.750: Tl, no Ca → low ✓  
- O10Ca2Cu3Ba2Tl2: Tl, has Ca → high ✓
- O7Cu3Ba2La1: La, no Ca → low ✓
- O7Cu3Y0.200Ba2La0.800: La, no Ca → low ✓
- All Hg-only materials (no Tl/La): high ✓
- All Ca-containing materials without Tl/La: high ✓

This rule fits all 21 measurable materials.

### Step C: Predictions for Unmeasurable Materials

Applying rule "Materials containing Tl or La are low Tc unless they also contain Ca":

1. O6.160Ca1Cu2Ba2Hg1: Ca present, no Tl/La → **high**
2. O6.270Ca1Cu2Ba2Hg1: Ca present, no Tl/La → **high**
3. O4.180Cu1Ba2Hg1: No Tl/La → **high**
4. O4.047Cu1Ba2Hg1: No Tl/La → **high**
5. O4.066Cu1Ba2Hg1: No Tl/La → **high**
6. O4.160Cu1Ba2Hg1: No Tl/La → **high**
7. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl AND Ca → **high**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl AND Ca → **high**
9. O8Cu3Y1Ba1.900La0.100: Contains La, no Ca → **low**
10. O8Cu4Y1Ba1.870La0.130: Contains La, no Ca → **low**

### Step D: Final Pick

Among predicted **high** materials, choose one with FEWEST elements (break ties by higher expected Tc).

**High materials with element counts:**
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements  
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Fewest elements:** O4 series (4 elements)

**Tie break (higher expected Tc):** Among O4.180, O4.047, O4.066, O4.160:
- O4.160 is closest to O4.162Cu1Ba2Hg1 (97.4 K, highest in O4 series)
- O4.180 is near O4.200 (94.2 K, lower)
- O4.047 and O4.066 have lower oxygen content, likely lower Tc based on O4.085 (92.0 K)

**Final choice:** O4.160Cu1Ba2Hg1

### Final Measurement Status
Used: 29 measurements
Remaining: 16 measurements