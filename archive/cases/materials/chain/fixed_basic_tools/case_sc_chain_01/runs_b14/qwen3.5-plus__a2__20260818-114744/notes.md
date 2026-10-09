# Superconductor Tc Study - Notes

## Step A: Find peak Tc in the homologous series

Seed: O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)

The homologous series is O6.xxxCa1Cu2Ba2Hg1 (same element set, varying oxygen).

### Measurements (O6.xxxCa1Cu2Ba2Hg1):
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.160: no sample (unmeasurable)
- O6.190: 122.0 K
- O6.210: 127.0 K  <- PEAK
- O6.220: 127.0 K  <- PEAK (tie)
- O6.240: 123.0 K
- O6.270: no sample (unmeasurable)
- O6.300: 100.0 K
- O6.310: 98.0 K

**T* = 127.0 K** (peak from O6.210Ca1Cu2Ba2Hg1)

**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter family B (contains Hg, exactly 4 elements)

threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials with Hg and exactly 4 elements are the O4.xxxCu1Ba2Hg1 series (elements: O, Cu, Ba, Hg).

### Measured O4.xxx values:
- O4.081: 98.0 K >= 95 ✓
- O4.150: 97.0 K >= 95 ✓
- O4.162: 97.4 K >= 95 ✓
- O4.220: 97.0 K >= 95 ✓

Pattern: All O4.xxxCu1Ba2Hg1 have Tc ~97-98 K, well above 95.

**Q1** = All 11 O4.xxxCu1Ba2Hg1 compositions from measurable.txt:
- O4.047Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

## Step C: Predict high/low for unmeasurable materials

Unmeasurable compositions (7 total):
1. O6.160Ca1Cu2Ba2Hg1 - has Hg, Ba -> high
2. O6.270Ca1Cu2Ba2Hg1 - has Hg, Ba -> high
3. O4.180Cu1Ba2Hg1 - has Hg, Ba -> high
4. O8Ca0.850Cu2.200Ba2Tl1.800 - has Tl, Ca -> high
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - has Tl, Ca -> high
6. O8Cu3Y1Ba1.900La0.100 - no Hg, no Tl -> low
7. O8Cu4Y1Ba1.870La0.130 - no Hg, no Tl -> low

**Induced Rule: High Tc if (has Hg AND has Ba) OR (has Tl AND has Ca).**

This rule is an interaction of multiple factors:
- Hg-containing materials with Ba are high (based on measured data)
- Tl-containing materials with Ca are also high (inferred from Tl being a known high-Tc superconductor element)
- Materials without Hg or Tl are low

**Predictions:**
- O6.160Ca1Cu2Ba2Hg1: high
- O6.270Ca1Cu2Ba2Hg1: high
- O4.180Cu1Ba2Hg1: high
- O8Ca0.850Cu2.200Ba2Tl1.800: high
- O7Ca1Cu2Sr2Tl0.500Pb0.500: high
- O8Cu3Y1Ba1.900La0.100: low
- O8Cu4Y1Ba1.870La0.130: low

## Step D: Final pick

Among unmeasurable materials predicted high, find the one with fewest elements.

High predictions:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
3. O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) <- FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among high predictions)
