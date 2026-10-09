# Notes for Superconductor Tc Study

## Step A - Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1
Homologous series: O6.xxxCa1Cu2Ba2Hg1 (lines 1-10 of unmeasurable.txt)

Measurements:
- O6.080: 92.0
- O6.100: 92.5
- O6.130: 102.0
- O6.140: 105.0
- O6.190: 122.0
- O6.210: 127.0 (MAX)
- O6.220: 127.0 (MAX)
- O6.240: 123.0
- O6.300: 100.0
- O6.310: 98.0

**T* = 127.0**, peak = O6.210Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O4.xxxCu1Ba2Hg1) with Tc >= 95.0:
- O4.047: 95.0 ✓
- O4.081: 98.0 ✓
- O4.096: 95.6 ✓
- O4.097: 95.3 ✓
- O4.150: 97.0 ✓
- O4.162: 97.4 ✓
- O4.220: 97.0 ✓
- O4.200: 94.2 ✗ (< 95.0)

Unmeasured (likely >= 95.0 based on trend):
- O4.101, O4.103, O4.085

Q1 = [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]

## Step C - Extrapolate
Rule: **Contains Cu AND (contains Hg OR contains Tl) → HIGH; otherwise LOW**

This is multi-factor: requires Cu AND (Hg or Tl).

Predictions for unmeasurable.txt:
- Lines 1-24 (have Cu+Hg or Cu+Tl): HIGH
- Lines 25-26 (have Cu but no Hg, no Tl): LOW

## Step D - Final Pick
Among HIGH predictions, find one with FEWEST elements.

4-element HIGH materials: Lines 11-21 (O4.xxxCu1Ba2Hg1)
Highest Tc among these: O4.081Cu1Ba2Hg1 (98.0 K)

**Final answer: O4.081Cu1Ba2Hg1**

---
