# Notes for Superconductor Tc Study

## Step A - Series Peak

Seed material: `O6.080Ca1Cu2Ba2Hg1`

The seed belongs to a homologous doping series with same element set (O, Ca, Cu, Ba, Hg), varying oxygen.

Measured series members (O6.xxCa1Cu2Ba2Hg1):
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.190: 122.0 K
- O6.210: 127.0 K (MAX)
- O6.220: 127.0 K (MAX)
- O6.240: 123.0 K
- O6.300: 100.0 K
- O6.310: 98.0 K

**T* = 127.0 K** (peak Tc)

Peak composition: **O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B - Filter Family B

threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Materials with Hg and exactly 4 elements (OxCu1Ba2Hg1):
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗

**Q1** = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

## Step C - Induced Rule

After analyzing measured data, the rule is:

**HIGH if (contains Ca) OR (4 elements AND contains Hg), otherwise LOW**

This is an interaction rule combining element count and element presence.

Predictions for all 26 unmeasurable materials:
- O6.080Ca1Cu2Ba2Hg1 through O6.310Ca1Cu2Ba2Hg1 (10 items): HIGH (have Ca)
- O4.081Cu1Ba2Hg1 through O4.085Cu1Ba2Hg1 (11 items): HIGH (4 elements, have Hg)
- O6Cu1Ba2Hg1Tl1: LOW (5 elements, no Ca)
- O10Ca2Cu3Ba2Tl2: HIGH (has Ca)
- O6Cu1Cd0.250Ba2Tl1.750: LOW (5 elements, no Ca)
- O7Cu3Ba2La1: LOW (4 elements, no Hg)
- O7Cu3Y0.200Ba2La0.800: LOW (5 elements, no Ca)

## Step D - Final Pick

Among HIGH predictions, find the one with fewest elements:
- 4-element HIGH materials: O4.xxxCu1Ba2Hg1 series (11 items)
- 5-element HIGH materials: O6.xxCa1Cu2Ba2Hg1 series (10 items) + O10Ca2Cu3Ba2Tl2

Among 4-element materials, highest Tc is O4.081Cu1Ba2Hg1 with 98.0 K.

**Final answer: O4.081Cu1Ba2Hg1**
