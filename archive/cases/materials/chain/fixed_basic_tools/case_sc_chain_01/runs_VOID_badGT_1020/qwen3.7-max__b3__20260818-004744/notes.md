# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- Measurable: 29 listed (25 had samples, 4 did not)
- Unmeasurable: 10 materials

## Step A - Measuring All Measurable Compositions
Measured 29 compositions. 4 had no sample:
- O6.160Ca1Cu2Ba2Hg1 (no sample)
- O6.270Ca1Cu2Ba2Hg1 (no sample)
- O4.180Cu1Ba2Hg1 (no sample)
- O4.047Cu1Ba2Hg1 (no sample)

### Measurement Results (25 with samples):

**O6.xxxCa1Cu2Ba2Hg1 series (5 elements: O,Ca,Cu,Ba,Hg):**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**O4.xxxCu1Ba2Hg1 series (4 elements: O,Cu,Ba,Hg):**
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

**Other materials:**
- O10Ca2Cu3Ba2Tl2 (5 elem: O,Ca,Cu,Ba,Tl): 118.97 K → HIGH
- O7Cu3Ba2La1 (4 elem: O,Cu,Ba,La): 89.6 K → LOW
- O6Cu1Cd0.250Ba2Tl1.750 (5 elem: O,Cu,Cd,Ba,Tl): 88.0 K → LOW
- O7Cu3Y0.200Ba2La0.800 (5 elem: O,Cu,Y,Ba,La): 81.0 K → LOW
- O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl): 0.0 K → LOW

**T* = 127.0 K (O6.210Ca1Cu2Ba2Hg1)**

## Step B - Filter Family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = 95.0

4-element Hg materials = O4.xxxCu1Ba2Hg1 series. Those with Tc >= 95.0:
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓

Q1 = 6 members

## Step C - Rule Induction

**HIGH (Tc >= 90) materials analysis:**
- All O4.xxxCu1Ba2Hg1: contain Hg, no Tl (92-98 K)
- All O6.xxxCa1Cu2Ba2Hg1: contain Hg, no Tl (92-127 K)
- O10Ca2Cu3Ba2Tl2: contains Ca (118.97 K)

**LOW (Tc < 90) materials analysis:**
- O6Cu1Ba2Hg1Tl1: has Hg AND Tl, no Ca → 0 K
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, has Tl, no Ca → 88 K
- O7Cu3Ba2La1: no Hg, no Ca → 89.6 K
- O7Cu3Y0.200Ba2La0.800: no Hg, no Ca → 81 K

**Rule: HIGH if (contains Hg AND does NOT contain Tl) OR (contains Ca)**

Verification: Works for ALL 25 measured materials.

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 (O,Ca,Cu,Ba,Hg): Hg+noTl → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (O,Ca,Cu,Ba,Hg): Hg+noTl → HIGH
3. O4.180Cu1Ba2Hg1 (O,Cu,Ba,Hg): Hg+noTl → HIGH
4. O4.047Cu1Ba2Hg1 (O,Cu,Ba,Hg): Hg+noTl → HIGH
5. O4.066Cu1Ba2Hg1 (O,Cu,Ba,Hg): Hg+noTl → HIGH
6. O4.160Cu1Ba2Hg1 (O,Cu,Ba,Hg): Hg+noTl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800 (O,Ca,Cu,Ba,Tl): Ca → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (O,Ca,Cu,Sr,Tl,Pb): Ca → HIGH
9. O8Cu3Y1Ba1.900La0.100 (O,Cu,Y,Ba,La): no Hg, no Ca → LOW
10. O8Cu4Y1Ba1.870La0.130 (O,Cu,Y,Ba,La): no Hg, no Ca → LOW

## Step D - Final Pick
HIGH predictions by element count:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Among 4-element tied ones, expected Tc from O4.xxxCu1Ba2Hg1 series:
- O4.066 closest to peak O4.081 (98.0 K) → highest expected Tc

**Final pick: O4.066Cu1Ba2Hg1**
