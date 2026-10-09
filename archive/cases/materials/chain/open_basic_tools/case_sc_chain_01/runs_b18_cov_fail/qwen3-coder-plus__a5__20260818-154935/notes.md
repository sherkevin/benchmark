# Superconductor Tc Study Notes

Starting date: 2026-08-18

Budget: 18 units total
- measure_tc() costs 1 unit
- search_db() costs 1 unit per page

Seed material: O6.080Ca1Cu2Ba2Hg1
High Tc definition: >= 90 K

## Step A - Series Peak
Goal: Find the composition with MAXIMUM Tc in the homologous doping series (same elements, varying oxygen)

First, I measured the seed material to establish a baseline:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (used 1 budget unit)

Found materials in the series:
Page 1: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1
Page 2: O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1
Page 3: O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1
Page 4: O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Tc = null (no sample available) - UNMEASURABLE
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAXIMUM Tc)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Tc = null (no sample available) - UNMEASURABLE
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Result of Step A:
- Maximum Tc (T*) = 127.0 K
- Peak compositions: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
Definition: Materials that contain Hg and have exactly 4 elements, with Tc >= threshold1
Where threshold1 = T* - 32 = 127.0 - 32 = 95.0

I searched for materials containing Hg and found:
Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
Page 4: O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1

These all have exactly 4 elements (O, Cu, Ba, Hg), so they qualify for Family B.
However, I used all my budget units and could not measure any of these materials.
Since I couldn't measure them, I cannot determine which ones have Tc >= 95.0.
Therefore, qualifying set Q1 is empty.

## Step C & D
I couldn't search further for additional unmeasurable materials due to budget constraints.
Based on the pattern from the measured materials in the series, I can make predictions:
- O6.160Ca1Cu2Ba2Hg1: This falls in the middle range of oxygen content, and given the bell-shaped curve of Tc vs oxygen content, it likely has a moderate Tc, probably < 90K, so "low"
- O6.270Ca1Cu2Ba2Hg1: This has oxygen content close to the peak region (6.21-6.22), so it might retain relatively high Tc, possibly > 90K, so "high"

For Step D, among the predicted high materials, O6.270Ca1Cu2Ba2Hg1 would be the choice as it has fewer elements (same number of elements as others, but higher expected Tc based on position in the series).

## Summary of my limitations
I used my entire budget (18 units) on Step A, leaving 0 units for additional measurements in Steps B, C, and D. In a better strategy, I would have reserved budget for measuring some of the Hg-containing 4-element materials to properly complete Step B.