# Notes: Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90K, Low: < 90K
- Budget: 14 measure_tc calls
- 4 steps: A (series peak), B (filter family B), C (extrapolate), D (final pick)

## Data Files
### Measurable (26 materials):
- Series 1 (O6.xxxCa1Cu2Ba2Hg1, 5 elements: O,Ca,Cu,Ba,Hg): 10 members (lines 1-10)
  - O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310
- Series 2 (O4.xxxCu1Ba2Hg1, 4 elements: O,Cu,Ba,Hg): 11 members (lines 11-21)
  - O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047, O4.200, O4.101, O4.103, O4.085
- Others (5 materials):
  - O6Cu1Ba2Hg1Tl1 (5 elements)
  - O10Ca2Cu3Ba2Tl2 (5 elements, no Hg)
  - O6Cu1Cd0.250Ba2Tl1.750 (5 elements, no Hg)
  - O7Cu3Ba2La1 (4 elements, no Hg)
  - O7Cu3Y0.200Ba2La0.800 (5 elements, no Hg)

### Unmeasurable (7 materials):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

## Step A: Series Peak
The seed's homologous series = O6.xxxCa1Cu2Ba2Hg1 (same element set {O,Ca,Cu,Ba,Hg}, varying oxygen)
Need to find max Tc among the 10 members.

### Measurements (7 of 10 series members measured):
| Composition | Tc (K) | Class |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

Unmeasured: O6.100 (~97K interpolated), O6.140 (~109K), O6.300 (~107K) — all below 127K.

**T* = 127.0K, Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter Family B (Hg + exactly 4 elements)
Materials with Hg and exactly 4 elements from measurable:
- O4.xxxCu1Ba2Hg1 series (11 members) - elements: O, Cu, Ba, Hg = 4
threshold1 = T* - 32 = 127.0 - 32 = **95.0K**

### O4.xxxCu1Ba2Hg1 Measurements (4 of 11):
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Dome is very flat (95.0 - 97.4K). All 4 measured members qualify.
Interpolated unmeasured members all >= 95K. **All 11 members qualify for Q1.**

## Additional Measurements (for rule induction):
| Composition | Tc (K) | Elements | Class |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | O,Cu,Ba,Hg,Tl (5) | LOW |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl (5) | HIGH |
| O7Cu3Ba2La1 | 89.6 | O,Cu,Ba,La (4) | LOW |

## Step C: Rule Induction

### Training Data Summary:
| Material | Contains Ca | Contains Hg | Contains Tl | Tc | Class |
|---|---|---|---|---|---|
| O6.xxxCa1Cu2Ba2Hg1 | ✓ | ✓ | ✗ | 92-127 | HIGH |
| O4.xxxCu1Ba2Hg1 | ✗ | ✓ | ✗ | 95-97 | HIGH |
| O10Ca2Cu3Ba2Tl2 | ✓ | ✗ | ✓ | 119 | HIGH |
| O6Cu1Ba2Hg1Tl1 | ✗ | ✓ | ✓ | 0 | LOW |
| O7Cu3Ba2La1 | ✗ | ✗ | ✗ | 89.6 | LOW |

### Induced Rule:
**HIGH if: (contains Ca) OR (contains Hg AND does NOT contain Tl)**

This is a multi-factor interaction rule. Key observations:
- Ca-containing materials are always HIGH (Ca enables multi-layer cuprate structures)
- Hg-containing materials are HIGH only without Tl (Tl+Hg without Ca breaks the structure)
- Materials without Ca or Hg are LOW

### Predictions for Unmeasurable Materials:
1. O6.160Ca1Cu2Ba2Hg1: has Ca → **HIGH** (~115K expected, between O6.130=102 and O6.190=122)
2. O6.270Ca1Cu2Ba2Hg1: has Ca → **HIGH** (~110K expected, between O6.240=123 and O6.310=98)
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → **HIGH** (~97K expected)
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → **HIGH** (~115K expected, similar to Tl-cuprates)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → **HIGH** (~100K expected)
6. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → **LOW** (~88K expected, similar to O7Cu3Ba2La1=89.6)
7. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → **LOW** (~87K expected)

## Step D: Final Pick
HIGH predictions and element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: **4 elements** (O,Cu,Ba,Hg) ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O,Ca,Cu,Sr,Tl,Pb)

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~97K)
