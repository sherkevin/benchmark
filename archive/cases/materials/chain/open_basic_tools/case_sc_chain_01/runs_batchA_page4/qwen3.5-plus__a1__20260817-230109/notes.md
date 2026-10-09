# Notes on Superconductor Tc Study

## Budget: 45 units total (all used)

## Seed Material: O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)

## Step A — Series Peak
The seed belongs to a homologous series with elements: O, Ca, Cu, Ba, Hg (5 elements)

Series members measured:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first occurrence of max)

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1: no sample
- O4.066Cu1Ba2Hg1: no sample
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 ✓
- O4.085Cu1Ba2Hg1: 92.0 K < 95.0
- O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 ✓
- O4.101Cu1Ba2Hg1: 93.4 K < 95.0
- O4.103Cu1Ba2Hg1: 92.5 K < 95.0
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.160Cu1Ba2Hg1: no sample
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 ✓
- O4.180Cu1Ba2Hg1: no sample
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 ✓

**Q1 (measurable materials with Tc >= 95.0):**
["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

## Step C — Extrapolate Rule
Induced rule from measurable materials:

**A material is LOW if it contains La, or Y, or Cd, or both Hg and Tl together. Otherwise it is HIGH.**

Verification:
- O6Cu1Ba2Hg1Tl1 (Hg+Tl): 0.0 K → low ✓
- O6Cu1Cd0.250Ba2Tl1.750 (Cd): 88.0 K → low ✓
- O7Cu3Ba2La1 (La): 89.6 K → low ✓
- O7Cu3Y0.200Ba2La0.800 (Y+La): 81.0 K → low ✓
- O10Ca2Cu3Ba2Tl2 (Tl only): 118.97 K → high ✓
- All Hg-only materials: 92-127 K → high ✓

**Unmeasurable materials and predictions:**
1. O4.047Cu1Ba2Hg1 (4 elem, Hg only) → high
2. O4.066Cu1Ba2Hg1 (4 elem, Hg only) → high
3. O4.160Cu1Ba2Hg1 (4 elem, Hg only) → high
4. O4.180Cu1Ba2Hg1 (4 elem, Hg only) → high
5. O6.160Ca1Cu2Ba2Hg1 (5 elem, Hg only) → high
6. O6.270Ca1Cu2Ba2Hg1 (5 elem, Hg only) → high
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, Tl only) → high
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, Tl only) → high
9. O8Cu3Y1Ba1.900La0.100 (5 elem, has Y+La) → low
10. O8Cu4Y1Ba1.870La0.130 (5 elem, has Y+La) → low

## Step D — Final Pick
Among unmeasurable materials predicted HIGH, choose the one with FEWEST elements.

High predictions with element counts:
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

Tie among 4-element materials. Break by higher expected Tc.
Based on proximity to measured values:
- O4.160 is closest to O4.162 (97.4 K), so highest expected Tc

**Final answer: O4.160Cu1Ba2Hg1**
