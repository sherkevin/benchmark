# Superconductor Tc Study Notes

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high if Tc >= 90 K, otherwise low
- Budget: 18 measure_tc calls

## Step A — Find series peak
Need to measure the homologous doping series (same elements, varying oxygen) to find max Tc.

## Step B — Filter family B
threshold1 = T* - 32
Find materials with Hg and exactly 4 elements, Tc >= threshold1

## Step C — Predict unmeasurables
Induce rule from measurable data to predict high/low for unmeasurables.

## Step D — Final pick
Among predicted high unmeasurables, pick one with fewest elements (tie-break: higher expected Tc).

---

## Measurements taken:

### Step A - Series members (O6.xxxCa1Cu2Ba2Hg1):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK, HIGH)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK tie, HIGH)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (HIGH)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (HIGH)

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

### Step B - Hg-containing 4-element materials (O4.xxxCu1Ba2Hg1):
- O4.180Cu1Ba2Hg1: no sample
- O4.081Cu1Ba2Hg1: 98.0 K (HIGH, >= 95.0, qualifies for Q1)
- O4.162Cu1Ba2Hg1: 97.4 K (HIGH, >= 95.0, qualifies)
- O4.150Cu1Ba2Hg1: 97.0 K (HIGH, >= 95.0, qualifies)
- O4.220Cu1Ba2Hg1: 97.0 K (HIGH, >= 95.0, qualifies)
- O4.096Cu1Ba2Hg1: 95.6 K (HIGH, >= 95.0, qualifies)
- O4.097Cu1Ba2Hg1: 95.3 K (HIGH, >= 95.0, qualifies)
- O4.047Cu1Ba2Hg1: 95.0 K (HIGH, >= 95.0, qualifies)
- O4.200Cu1Ba2Hg1: 94.2 K (HIGH but < 95.0, does NOT qualify)
- O4.101Cu1Ba2Hg1: not measured (budget exhausted)
- O4.103Cu1Ba2Hg1: not measured
- O4.085Cu1Ba2Hg1: not measured

**Q1 (materials with Hg, 4 elements, Tc >= 95.0):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.220Cu1Ba2Hg1 (97.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.047Cu1Ba2Hg1 (95.0)

### Step C - Predictions for unmeasurable.txt:
Rule: HIGH if contains (Hg OR Tl) AND Cu AND Ba; otherwise LOW.

Predictions:
- O6.080-0.310Ca1Cu2Ba2Hg1 (10 materials): HIGH (measured, contain Hg, Cu, Ba)
- O4.081-0.220Cu1Ba2Hg1 (11 materials): HIGH (measured or predicted, contain Hg, Cu, Ba)
- O4.101, O4.103, O4.085Cu1Ba2Hg1: HIGH (predicted, contain Hg, Cu, Ba)
- O6Cu1Ba2Hg1Tl1: HIGH (contains Hg, Cu, Ba)
- O10Ca2Cu3Ba2Tl2: HIGH (contains Tl, Cu, Ba)
- O6Cu1Cd0.250Ba2Tl1.750: HIGH (contains Tl, Cu, Ba)
- O7Cu3Ba2La1: LOW (no Hg or Tl)
- O7Cu3Y0.200Ba2La0.800: LOW (no Hg or Tl)

### Step D - Final pick:
Among predicted HIGH unmeasurables, find one with fewest elements.

HIGH predictions:
- O6.080-0.310Ca1Cu2Ba2Hg1: 5 elements each
- O4.xxxCu1Ba2Hg1 (11 materials): 4 elements each
- O6Cu1Ba2Hg1Tl1: 5 elements
- O10Ca2Cu3Ba2Tl2: 5 elements
- O6Cu1Cd0.250Ba2Tl1.750: 5 elements

HIGH predictions with 4 elements (fewest): O4.101, O4.103, O4.085Cu1Ba2Hg1 (and other measured O4.xxx)
Among unmeasured 4-element HIGH: O4.101, O4.103, O4.085
Tie-break by expected Tc: O4.085 estimated ~97 K (highest, near O4.081's 98.0)

**Final answer: O4.085Cu1Ba2Hg1**
