# Notes on Superconductor Tc Study

## Step A — Series Peak (DONE)

Seed: O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)

Homologous series (same elements O, Ca, Cu, Ba, Hg with varying oxygen):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <- Also peak (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

Budget used: 14/14 (all used)

## Step B — Filter Family B

threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements: O4.xxxCu1Ba2Hg1 series (elements: O, Cu, Ba, Hg)

Measured:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 ✓

All measured values are >= 95.0 K, so all 11 O4.xxxCu1Ba2Hg1 compositions qualify.

**Q1 = all 11 O4.xxxCu1Ba2Hg1 compositions**

## Step C — Induced Rule

All measured materials with Hg AND Cu have HIGH Tc (>= 90 K).
Materials without Hg are predicted LOW.

**Rule: HIGH if material contains both Hg AND Cu; LOW otherwise.**

Predictions for all 26 unmeasurable.txt compositions:
- O6.080Ca1Cu2Ba2Hg1: HIGH (measured 92.0)
- O6.100Ca1Cu2Ba2Hg1: HIGH (measured 92.5)
- O6.130Ca1Cu2Ba2Hg1: HIGH (measured 102.0)
- O6.140Ca1Cu2Ba2Hg1: HIGH (measured 105.0)
- O6.190Ca1Cu2Ba2Hg1: HIGH (measured 122.0)
- O6.210Ca1Cu2Ba2Hg1: HIGH (measured 127.0)
- O6.220Ca1Cu2Ba2Hg1: HIGH (measured 127.0)
- O6.240Ca1Cu2Ba2Hg1: HIGH (measured 123.0)
- O6.300Ca1Cu2Ba2Hg1: HIGH (measured 100.0)
- O6.310Ca1Cu2Ba2Hg1: HIGH (measured 98.0)
- O4.081Cu1Ba2Hg1: HIGH (measured 98.0)
- O4.162Cu1Ba2Hg1: HIGH (measured 97.4)
- O4.150Cu1Ba2Hg1: HIGH (measured 97.0)
- O4.220Cu1Ba2Hg1: HIGH (measured 97.0)
- O4.096Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.097Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.047Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.200Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.101Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.103Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O4.085Cu1Ba2Hg1: HIGH (predicted - has Hg, Cu)
- O6Cu1Ba2Hg1Tl1: HIGH (predicted - has Hg, Cu)
- O10Ca2Cu3Ba2Tl2: LOW (predicted - no Hg)
- O6Cu1Cd0.250Ba2Tl1.750: LOW (predicted - no Hg)
- O7Cu3Ba2La1: LOW (predicted - no Hg)
- O7Cu3Y0.200Ba2La0.800: LOW (predicted - no Hg)

## Step D — Final Pick

Among predicted HIGH materials, find the one with FEWEST elements.

Predicted HIGH (not measured):
- O4.096Cu1Ba2Hg1: 4 elements
- O4.097Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.200Cu1Ba2Hg1: 4 elements
- O4.101Cu1Ba2Hg1: 4 elements
- O4.103Cu1Ba2Hg1: 4 elements
- O4.085Cu1Ba2Hg1: 4 elements
- O6Cu1Ba2Hg1Tl1: 5 elements

All O4.xxxCu1Ba2Hg1 have 4 elements (tie for fewest).

To break tie, use higher expected Tc. Based on measured O4.xxx data:
- O4.081: 98.0 K (highest)
- O4.150: 97.0 K
- O4.162: 97.4 K
- O4.220: 97.0 K

O4.085 is closest to O4.081 (which had highest Tc), so O4.085Cu1Ba2Hg1 should have highest expected Tc.

**Final answer: O4.085Cu1Ba2Hg1**
