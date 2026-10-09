# Notes for Superconductor Tc Study

## Task Summary
- Seed: O6.080Ca1Cu2Ba2Hg1 (elements: O, Ca, Cu, Ba, Hg - 5 elements)
- Budget: 18 measure_tc calls (exhausted)
- Definition: high if Tc >= 90 K, otherwise low

## Step A — Series Peak (T*)
Homologous series: same elements as seed (O, Ca, Cu, Ba, Hg), varying oxygen.

Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← MAX
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← MAX (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first with max Tc)

## Step B — Filter Family B (Hg + exactly 4 elements, Tc >= threshold1)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

4-element Hg materials measured:
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓

**Q1** = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C — Predictions for Unmeasurable Materials
Induced rule: **HIGH if contains Ba AND (Hg OR Ca)**

This rule correctly predicts:
- All O6.xxxCa1Cu2Ba2Hg1: have Ba, Hg, Ca → HIGH
- All O4.xCu1Ba2Hg1: have Ba, Hg → HIGH
- O10Ca2Cu3Ba2Tl2: has Ba, Ca → HIGH
- O6Cu1Ba2Hg1Tl1: has Ba, Hg → HIGH
- O6Cu1Cd0.250Ba2Tl1.750: has Ba, no Hg, no Ca → LOW
- O7Cu3Ba2La1: has Ba, no Hg, no Ca → LOW
- O7Cu3Y0.200Ba2La0.800: has Ba, no Hg, no Ca → LOW

Predictions for all unmeasurable.txt:
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
- O4.096Cu1Ba2Hg1: HIGH (measured 95.6)
- O4.097Cu1Ba2Hg1: HIGH (measured 95.3)
- O4.047Cu1Ba2Hg1: HIGH (measured 95.0)
- O4.200Cu1Ba2Hg1: HIGH (measured 94.2)
- O4.101Cu1Ba2Hg1: HIGH (predicted: has Hg, Ba)
- O4.103Cu1Ba2Hg1: HIGH (predicted: has Hg, Ba)
- O4.085Cu1Ba2Hg1: HIGH (predicted: has Hg, Ba)
- O6Cu1Ba2Hg1Tl1: HIGH (predicted: has Hg, Ba)
- O10Ca2Cu3Ba2Tl2: HIGH (predicted: has Ca, Ba)
- O6Cu1Cd0.250Ba2Tl1.750: LOW (predicted: no Hg, no Ca)
- O7Cu3Ba2La1: LOW (predicted: no Hg, no Ca)
- O7Cu3Y0.200Ba2La0.800: LOW (predicted: no Hg, no Ca)

## Step D — Final Pick
Among HIGH predictions, find fewest elements, then highest Tc.

4-element HIGH materials:
- O4.047Cu1Ba2Hg1: 95.0 K
- O4.081Cu1Ba2Hg1: 98.0 K ← highest
- O4.085Cu1Ba2Hg1: ? (predicted HIGH)
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.101Cu1Ba2Hg1: ? (predicted HIGH)
- O4.103Cu1Ba2Hg1: ? (predicted HIGH)
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.220Cu1Ba2Hg1: 97.0 K

**Final answer: O4.081Cu1Ba2Hg1** (4 elements, highest measured Tc = 98.0 K)
