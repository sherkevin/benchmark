# Notes for Superconductor Tc Study

Budget: 45 units total (used all 45)

## Step A — Series Peak
Seed: `O6.080Ca1Cu2Ba2Hg1`
Homologous series: O6.xxxCa1Cu2Ba2Hg1 (same elements, varying oxygen)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.160Ca1Cu2Ba2Hg1: no sample (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 ← PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.270Ca1Cu2Ba2Hg1: no sample (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O4.xxxCu1Ba2Hg1):
- O4.047: no sample
- O4.066: no sample
- O4.081: 98.0 >= 95 ✓
- O4.085: 92.0 < 95 ✗
- O4.096: 95.6 >= 95 ✓
- O4.097: 95.3 >= 95 ✓
- O4.101: 93.4 < 95 ✗
- O4.103: 92.5 < 95 ✗
- O4.150: 97.0 >= 95 ✓
- O4.160: no sample
- O4.162: 97.4 >= 95 ✓
- O4.180: no sample
- O4.200: 94.2 < 95 ✗
- O4.220: 97.0 >= 95 ✓

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

## Step C — Extrapolate
Unmeasurable materials found:
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.160Cu1Ba2Hg1
4. O4.180Cu1Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.270Ca1Cu2Ba2Hg1
7. O7Ca1Cu2Sr2Tl0.500Pb0.500
8. O8Ca0.850Cu2.200Ba2Tl1.800
9. O8Cu3Y1Ba1.900La0.100
10. O8Cu4Y1Ba1.870La0.130

**Induced Rule:** Materials containing both Hg AND Tl have low Tc; all others have high Tc.

Evidence:
- O6Cu1Ba2Hg1Tl1 (has both Hg and Tl): Tc = 0.0 (LOW)
- All materials with only Hg (no Tl): HIGH
- O10Ca2Cu3Ba2Tl2 (has Tl, no Hg): Tc = 118.97 (HIGH)

**Predictions:**
- O4.047Cu1Ba2Hg1: HIGH (Hg, no Tl)
- O4.066Cu1Ba2Hg1: HIGH (Hg, no Tl)
- O4.160Cu1Ba2Hg1: HIGH (Hg, no Tl)
- O4.180Cu1Ba2Hg1: HIGH (Hg, no Tl)
- O6.160Ca1Cu2Ba2Hg1: HIGH (Hg, no Tl)
- O6.270Ca1Cu2Ba2Hg1: HIGH (Hg, no Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (Tl, no Hg)
- O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (Tl, no Hg)
- O8Cu3Y1Ba1.900La0.100: HIGH (no Hg, no Tl)
- O8Cu4Y1Ba1.870La0.130: HIGH (no Hg, no Tl)

## Step D — Final Pick
Among unmeasurable predicted HIGH, find one with FEWEST elements:

Element counts:
- O4.047Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O8Cu3Y1Ba1.900La0.100: 5 elements
- O8Cu4Y1Ba1.870La0.130: 5 elements

Tie among 4-element materials. Break by higher expected Tc.
Based on nearby measured values in O4 series:
- O4.162 has Tc=97.4 (peak)
- O4.160 is closest to this peak

**Final: O4.160Cu1Ba2Hg1**
