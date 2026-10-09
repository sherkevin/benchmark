# Superconductor Tc Study Notes

## Overview
- Task: Find optimal superconducting materials with limited measurement budget (18 measurements)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc defined as >= 90K
- Need to complete 4 dependent steps

## Data Analysis
- measurable.txt: 27 materials total
  - Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 series (homologous doping series with varying oxygen)
  - Lines 11-21: O4.xxxCu1Ba2Hg1 series (another series with varying oxygen)
  - Lines 22-27: Various other compositions

- unmeasurable.txt: 7 materials that cannot be measured (no physical samples)

## Step A: Series Peak
Goal: Measure homologous doping series (same element set, varying oxygen) from data/measurable.txt
Find composition with maximum Tc (T*)
Focus on lines 1-10: O6.xxxCa1Cu2Ba2Hg1 series (10 materials)

### Measurements:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (also maximum)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**Result:** T* = 127.0 K from O6.210Ca1Cu2Ba2Hg1
**Threshold1:** T* - 32 = 127.0 - 32 = 95.0 K

## Step B: Filter Family B
Find materials containing Hg with exactly 4 elements where Tc >= threshold1 (95.0 K)

### Eligible materials (contain Hg, have exactly 4 elements):
From measurable.txt lines 11-21: O4.xxxCu1Ba2Hg1 series (11 materials)

### Measurements (8 measurements used):
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (qualifies)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ (qualifies)
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ (qualifies)
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ (qualifies)
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ (qualifies)
6. O4.097Cu1Ba2Hg1: 95.3 K ✓ (qualifies)
7. O4.047Cu1Ba2Hg1: 95.0 K ✓ (qualifies)
8. O4.200Cu1Ba2Hg1: 94.2 K ✗ (does NOT qualify)

**Note:** Used all 18 measurement budget. Still need to check:
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1  
- O4.085Cu1Ba2Hg1

But no budget left. Need to proceed with Step C using available data.

## Step B: Filter Family B
Find materials containing Hg with exactly 4 elements where Tc >= threshold1 (T* - 32)
Need to identify which materials contain Hg and have exactly 4 elements

## Step C: Extrapolate Rule
Induce rule separating high/low Tc using measurable materials
Predict high/low for unmeasurable materials in data/unmeasurable.txt

## Step D: Final Pick
Among predicted high unmeasurable materials, pick one with fewest elements (tie break: higher expected Tc)

## Measurement Budget: 18 calls available
Strategy: Need to be efficient with measurements