# Notes: Superconductor Tc Study

## Step A: Homologous Series Results (O-Ca-Cu-Ba-Hg)
| Composition | Tc (K) | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | HIGH |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK (tied) |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1, first occurrence)

## Step B: 4-element Hg materials (O-Cu-Ba-Hg)
threshold1 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | Qualifies? |
|---|---|---|
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

**Q1 = 7 materials** (all with Tc >= 95.0)

## Additional measurements for rule induction
| Composition | Tc (K) | High/Low | Elements |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW | O,Cu,Ba,Hg,Tl (5) |
| O7Cu3Ba2La1 | 89.6 | LOW | O,Cu,Ba,La (4) |
| O10Ca2Cu3Ba2Tl2 | 118.97 | HIGH | O,Ca,Cu,Ba,Tl (5) |

## Step C: Rule Induction

### HIGH materials (all measured):
- O6.xxx Ca1Cu2Ba2Hg1 (92-127 K): has Ba + Ca + Hg, no Tl
- O4.xxx Cu1Ba2Hg1 (92-98 K): has Ba + Hg, no Ca, no Tl
- O10Ca2Cu3Ba2Tl2 (119 K): has Ba + Ca + Tl, no Hg

### LOW materials (all measured):
- O6Cu1Ba2Hg1Tl1 (0 K): has Ba + Hg + Tl (BOTH Hg and Tl), no Ca
- O7Cu3Ba2La1 (89.6 K): has Ba + La, no Ca/Hg/Tl

### Induced Rule:
**HIGH if (Ba AND Hg AND NOT Tl) OR (Ba AND Ca)**
Equivalently: Must contain Ba, AND must have either (Hg without Tl) or Ca.

Key observations:
- Having both Hg and Tl together kills superconductivity (O6Cu1Ba2Hg1Tl1 = 0 K)
- Having Ba but none of Ca/Hg/Tl gives low Tc (O7Cu3Ba2La1 = 89.6 K)
- Ca can compensate: Ca+Ba+Tl is HIGH even without Hg (O10Ca2Cu3Ba2Tl2 = 119 K)

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Ba✓, Ca✓, Hg✓, Tl✗ → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Ba✓, Ca✓, Hg✓, Tl✗ → HIGH
3. O4.180Cu1Ba2Hg1: Ba✓, Hg✓, Tl✗ → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba✓, Ca✓, Tl✓, Hg✗ → HIGH (Ca present)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ba✗ → LOW (no Ba)
6. O8Cu3Y1Ba1.900La0.100: Ba✓, but no Ca/Hg/Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: Ba✓, but no Ca/Hg/Tl → LOW

## Step D: Final Pick
HIGH predictions with element counts:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~95-96 K based on O4 series interpolation)
