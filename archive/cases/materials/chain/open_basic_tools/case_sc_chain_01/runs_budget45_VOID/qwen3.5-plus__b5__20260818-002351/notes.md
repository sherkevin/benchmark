# Superconductor Tc Study Notes

## Budget: 45 units total
Final budget used: 40 (14 search + 26 measure)

## Step A - Series peak (COMPLETE)
Seed: O6.080Ca1Cu2Ba2Hg1
Homologous series: O_x Ca1 Cu2 Ba2 Hg1

Series members measured:
- O6.080: 92.0 (low)
- O6.100: 92.5 (low)
- O6.130: 102.0 (high)
- O6.140: 105.0 (high)
- O6.160: NO SAMPLE
- O6.190: 122.0 (high)
- O6.210: 127.0 (high) <-- PEAK
- O6.220: 127.0 (high) <-- PEAK (tie)
- O6.240: 123.0 (high)
- O6.270: NO SAMPLE
- O6.300: 100.0 (high)
- O6.310: 98.0 (high)

**T* = 127.0 K** (peak: O6.210Ca1Cu2Ba2Hg1)

## Step B - Filter family B (COMPLETE)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials with Hg AND exactly 4 elements (O-Cu-Ba-Hg series):
- O4.047: NO SAMPLE
- O4.066: NO SAMPLE
- O4.081: 98.0 >= 95 ✓ Q1
- O4.085: 92.0 < 95 ✗
- O4.096: 95.6 >= 95 ✓ Q1
- O4.097: 95.3 >= 95 ✓ Q1
- O4.101: 93.4 < 95 ✗
- O4.103: 92.5 < 95 ✗
- O4.150: 97.0 >= 95 ✓ Q1
- O4.160: NO SAMPLE
- O4.162: 97.4 >= 95 ✓ Q1
- O4.180: NO SAMPLE
- O4.200: 94.2 < 95 ✗
- O4.220: 97.0 >= 95 ✓ Q1

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

## Step C - Predictions for unmeasurable materials (COMPLETE)
Unmeasurable materials found:
1. O4.047Cu1Ba2Hg1 (4 elements) - predicted LOW (underdoped)
2. O4.066Cu1Ba2Hg1 (4 elements) - predicted LOW (underdoped)
3. O4.160Cu1Ba2Hg1 (4 elements) - predicted HIGH (in optimal 0.15-0.16 region)
4. O4.180Cu1Ba2Hg1 (4 elements) - predicted LOW (transition region toward 0.200)
5. O6.160Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH (O > 0.13)
6. O6.270Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH (O > 0.13)

**Rule induced:** Tc >= 95 when oxygen content corresponds to optimal hole doping:
- For 5-element O-Ca-Cu-Ba-Hg: O >= 0.13 gives high Tc
- For 4-element O-Cu-Ba-Hg: specific oxygen levels (~0.08, ~0.096-0.097, ~0.15-0.16, ~0.22) give high Tc

## Step D - Final pick (COMPLETE)
Among unmeasurables predicted HIGH:
- O4.160Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

**Final answer: O4.160Cu1Ba2Hg1** (fewest elements = 4)
