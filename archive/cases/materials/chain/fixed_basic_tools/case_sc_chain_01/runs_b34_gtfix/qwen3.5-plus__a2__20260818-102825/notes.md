# Superconductor Tc Study Notes

## Step A — Series Peak

Seed: O6.080Ca1Cu2Ba2Hg1

The homologous series is O_xCa1Cu2Ba2Hg1 with varying oxygen content.

### Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← MAX (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (also max)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter Family B (Hg + exactly 4 elements, Tc >= threshold1)

threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements (O_xCu1Ba2Hg1 series):
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗

**Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]**

## Step C — Extrapolate (induce rule for unmeasurable materials)

### All measured materials summary:

**HIGH (Tc >= 90):**
- O_xCa1Cu2Ba2Hg1 series: all have Ca and Hg, no Tl
- O_xCu1Ba2Hg1 series: all have Hg, no Tl
- O10Ca2Cu3Ba2Tl2: has Ca, no La

**LOW (Tc < 90):**
- O6Cu1Ba2Hg1Tl1: has Hg AND Tl, no Ca → 0.0
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, no Ca → 88.0
- O7Cu3Ba2La1: has La, no Ca, no Hg → 89.6
- O7Cu3Y0.200Ba2La0.800: has La, no Ca, no Hg → 81.0

### Induced Rule:
**HIGH if: (has Hg AND NOT has Tl) OR (has Ca AND NOT has La)**
**LOW otherwise**

This is an interaction rule: single factors like "has Hg" or "has Ca" alone don't work because:
- Hg + Tl together (without Ca) → LOW (poisoning effect)
- La presence without Ca → LOW
- Tl without Ca or Hg → LOW

### Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: has Ca, has Hg, no Tl → (Hg and NOT Tl)=true → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: has Ca, has Hg, no Tl → **HIGH**
3. O4.180Cu1Ba2Hg1: has Hg, no Tl → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca, no La → (Ca and NOT La)=true → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca, no La → **HIGH**
6. O8Cu3Y1Ba1.900La0.100: has La, no Ca, no Hg → **LOW**
7. O8Cu4Y1Ba1.870La0.130: has La, no Ca, no Hg → **LOW**

## Step D — Final Pick

Among HIGH predictions, find fewest elements:

- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, the fewest among HIGH predictions)

## Budget Used
26 out of 34 measurements
