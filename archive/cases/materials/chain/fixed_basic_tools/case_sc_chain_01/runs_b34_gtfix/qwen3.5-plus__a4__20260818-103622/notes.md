# Notes on Superconductor Tc Study

## Step A — Series Peak

Seed: `O6.080Ca1Cu2Ba2Hg1`

The homologous series has same element set (O, Ca, Cu, Ba, Hg) with varying oxygen.

### Measurements (Step A):
| Composition | Tc |
|-------------|-----|
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

**T* = 127.0 K** (maximum)
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B — Filter Family B

Threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Materials with Hg and exactly 4 elements (O, Cu, Ba, Hg):
| Composition | Tc | >= 95.0? |
|-------------|-----|---------|
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

**Q1 (qualifying):**
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

## Step C — Induced Rule

After analyzing measured data, the rule that separates HIGH (Tc >= 90) from LOW (Tc < 90) is:

**HIGH if (Has Hg AND NOT Has Tl) OR (Has Ca AND Has Tl AND NOT Has Hg)**

This rule correctly predicts all measured materials:
- Ca-Cu-Ba-Hg-O series: Has Hg, no Tl → HIGH ✓
- Cu-Ba-Hg-O (4-element): Has Hg, no Tl → HIGH ✓
- O10Ca2Cu3Ba2Tl2: Has Ca, Tl, no Hg → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Has both Hg AND Tl → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: Has Tl, no Hg, no Ca → LOW ✓
- O7Cu3Ba2La1: No Hg, no Tl → LOW ✓
- O7Cu3Y0.200Ba2La0.800: No Hg, no Tl → LOW ✓

### Predictions for unmeasurable.txt:
All Ca-Cu-Ba-Hg-O series (10 items): HIGH
All Cu-Ba-Hg-O (11 items): HIGH
O6Cu1Ba2Hg1Tl1: LOW
O10Ca2Cu3Ba2Tl2: HIGH
O6Cu1Cd0.250Ba2Tl1.750: LOW
O7Cu3Ba2La1: LOW
O7Cu3Y0.200Ba2La0.800: LOW

## Step D — Final Pick

Among HIGH predictions, find the one with FEWEST elements:
- 4-element HIGH: Cu-Ba-Hg-O series (O4.xxxCu1Ba2Hg1)
- 5-element HIGH: Ca-Cu-Ba-Hg-O series and O10Ca2Cu3Ba2Tl2

Among 4-element HIGH, highest Tc is **O4.081Cu1Ba2Hg1** with Tc=98.0.

**Final answer: O4.081Cu1Ba2Hg1**
