# Notes on Superconductor Tc Study

## Step A — Series Peak
Measured the homologous series O6.xxxCa1Cu2Ba2Hg1 and O4.xxxCu1Ba2Hg1.

**O6.xxxCa1Cu2Ba2Hg1 series results:**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O6.160, O6.270: no sample available

**O4.xxxCu1Ba2Hg1 series results:**
- O4.047Cu1Ba2Hg1: 95.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.162, O4.200, O4.220: not measured (budget exhausted)
- O4.180: no sample available

**T* = 127.0 K** (maximum Tc, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

Peak composition: **O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Materials with Hg and exactly 4 elements (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓ QUALIFIES
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓ QUALIFIES
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓ QUALIFIES
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓ QUALIFIES
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓ QUALIFIES

Q1 = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1}

## Step C — Extrapolate Rule
Induced rule: **HIGH if (contains Hg) OR (contains Tl AND contains Ca)**

This is an interaction rule (not single-factor) because it requires the combination of Tl AND Ca for non-Hg materials.

Predictions for unmeasurable.txt:
- All O6.xxxCa1Cu2Ba2Hg1: HIGH (contain Hg)
- All O4.xxxCu1Ba2Hg1: HIGH (contain Hg)
- O6Cu1Ba2Hg1Tl1: HIGH (contains Hg)
- O10Ca2Cu3Ba2Tl2: HIGH (contains Tl AND Ca)
- O6Cu1Cd0.250Ba2Tl1.750: LOW (no Hg, no Ca)
- O7Cu3Ba2La1: LOW (no Hg, no Tl)
- O7Cu3Y0.200Ba2La0.800: LOW (no Hg, no Tl)

## Step D — Final Pick
Among HIGH predictions, find the one with fewest elements:
- O4.xxxCu1Ba2Hg1 series: 4 elements (O, Cu, Ba, Hg)
- O6.xxxCa1Cu2Ba2Hg1 series: 5 elements
- Others: 5+ elements

Among 4-element HIGH materials, the one with highest expected Tc is **O4.081Cu1Ba2Hg1** (measured 98.0 K).

**Final answer: O4.081Cu1Ba2Hg1**
