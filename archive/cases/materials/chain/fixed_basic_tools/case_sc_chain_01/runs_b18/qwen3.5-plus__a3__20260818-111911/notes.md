# Notes for Superconductor Tc Study

## Seed Material
- O6.080Ca1Cu2Ba2Hg1 (Tc=92.0)

## Task Overview
- Budget: 18 measure_tc calls (EXHAUSTED)
- Step A: Find max Tc in homologous series
- Step B: Find Q1 (Hg-containing, 4 elements, Tc >= T* - 32)
- Step C: Predict high/low for unmeasurable materials
- Step D: Pick unmeasurable high with fewest elements

## Measurements Summary
Measured 16 unique compositions. Key findings:
- O6.080Ca1Cu2Ba2Hg1: 92.0 (seed, homologous series)
- O6.100Ca1Cu2Ba2Hg1: 92.5 (homologous series)
- O4.081Cu1Ba2Hg1: 98.0 (3 elements: Cu, Ba, Hg)
- O10Ca2Cu3Ba2Tl2: 118.97 (4 elements: Ca, Cu, Ba, Tl)
- O6Cu1Ba2Hg1Tl1: 0.0 (LOW)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 (LOW)

## Induced Rule
**High Tc if (contains Ca) OR (contains Hg AND NOT contains Tl)**

Verification:
- Materials with Ca: HIGH (regardless of Hg/Tl)
- Materials with Hg but no Tl: HIGH
- Materials with no Ca, no Hg, has Tl: LOW
- Materials with no Ca, has Hg, has Tl: LOW

## Step A - Series Peak
Homologous series (Ca, Cu, Ba, Hg) members measured:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5

Peak T* = 92.5 (O6.100Ca1Cu2Ba2Hg1)

## Step B - Q1
threshold1 = 92.5 - 32 = 60.5

Q1 (Hg, 4 elements, Tc >= 60.5):
- O6.080Ca1Cu2Ba2Hg1 (Tc=92.0)
- O6.100Ca1Cu2Ba2Hg1 (Tc=92.5)

## Step C - Predictions
Using rule: High if (Ca) OR (Hg AND NOT Tl)

HIGH predictions (have Ca or Hg without Tl):
- Lines 1-10: All have Ca and Hg, no Tl → HIGH
- Lines 11-21: All have Hg, no Ca, no Tl → HIGH
- Line 23: Has Ca → HIGH

LOW predictions (no Ca, and either no Hg or has Tl):
- Line 22: No Ca, has Hg, has Tl → LOW
- Line 24: No Ca, no Hg, has Tl → LOW
- Lines 25-26: No Ca, no Hg, no Tl → LOW

## Step D - Final Pick
Among HIGH predictions, find fewest elements:
- Lines 11-21 (O4.xxxCu1Ba2Hg1): 3 elements (Cu, Ba, Hg) - FEWEST
- Lines 1-10, 23: 4 elements

Among 3-element HIGH compositions, highest Tc is O4.081Cu1Ba2Hg1 (98.0).

**Final answer: O4.081Cu1Ba2Hg1**
