# Superconductor Tc Study Notes

## Budget: 18 units total (EXHAUSTED)

## Step A - Series peak (COMPLETE)
Seed: O6.080Ca1Cu2Ba2Hg1 (Tc=92.0)
Homologous series: O_xCa1Cu2Ba2Hg1 (same element set, varying oxygen)

Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <- PEAK (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <- PEAK (T*)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B - Filter family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Family B: contains Hg AND exactly 4 elements

Known 4-element Hg materials (from Hg page 1):
- O4.047Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg) - NOT MEASURED
- O4.066Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg) - NOT MEASURED
- O4.081Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg) - NOT MEASURED

Budget exhausted before measuring these. Cannot determine Q1.

## Step C - Extrapolate
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1 (5 elements) - no sample
- O6.270Ca1Cu2Ba2Hg1 (5 elements) - no sample

Predictions based on trend:
- O6.160: Between O6.140 (105) and O6.190 (122), expect ~110-115 -> HIGH
- O6.270: Between O6.240 (123) and O6.300 (100), expect ~110 -> HIGH

Rule induced: Tc follows a parabolic trend with oxygen content, peaking around O6.210-O6.220.
Materials near the peak oxygen level have highest Tc.

## Step D - Final pick
Among unmeasurable materials predicted "high":
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Both have 5 elements (tie). Higher expected Tc:
- O6.270 is closer to peak (O6.210-O6.220) than O6.160
- Actually O6.160 is 0.05 away from 0.210, O6.270 is 0.05 away from 0.220
- Both are roughly equidistant from peak

Looking at measured values:
- O6.140 (dist 0.07 from 0.210): 105
- O6.240 (dist 0.02 from 0.220): 123

O6.270 would be at dist 0.05 from 0.220, O6.160 at dist 0.05 from 0.210.
Based on symmetry, O6.270 might have slightly higher Tc (the decline after peak seems slower).

Actually, looking more carefully:
- O6.220 to O6.240 (delta 0.02): 127 -> 123 (drop of 4)
- O6.240 to O6.300 (delta 0.06): 123 -> 100 (drop of 23)
- O6.300 to O6.310 (delta 0.01): 100 -> 98 (drop of 2)

For O6.270 (between 0.240 and 0.300):
Interpolating: at 0.270, roughly 123 - (0.03/0.06)*23 = 123 - 11.5 = 111.5

For O6.160 (between 0.140 and 0.190):
Interpolating: at 0.160, roughly 105 + (0.02/0.05)*17 = 105 + 6.8 = 111.8

They're very close. O6.160 might be slightly higher.

But wait - I may have missed other unmeasurable materials in the database. I only searched for Ca-containing materials thoroughly.

## Issue
I exhausted my budget measuring the homologous series and didn't leave budget for:
1. Measuring 4-element Hg materials for Step B
2. Searching for other unmeasurable materials

The answer may be incomplete.
