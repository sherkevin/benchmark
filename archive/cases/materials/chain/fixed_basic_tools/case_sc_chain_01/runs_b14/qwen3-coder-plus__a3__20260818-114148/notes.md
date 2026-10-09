# Notes for Superconductor Tc Study

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 14 measurements
- Definition: High Tc >= 90 K, Low Tc < 90 K

## Step A - Series Peak
Measured Tc values for the compositions in measurable.txt:

Series 1 (O-Ca-Cu-Ba-Hg):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

Series 2 (O-Cu-Ba-Hg):
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K

Other compositions (ran out of measurement budget):
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

MAXIMUM Tc found: 127.0 K for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
So T* = 127.0 K, and the peak composition is either O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1

## Step B - Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Materials that contain Hg and have exactly 4 elements from our measurements:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓ (4 elements: Cu, Ba, Hg, O)
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓ (4 elements: Cu, Ba, Hg, O)
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓ (4 elements: Cu, Ba, Hg, O)
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓ (4 elements: Cu, Ba, Hg, O)

For the Ca-Cu-Ba-Hg-O series, we have 5 elements, so they don't qualify for family B.

So Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C & D - Need more data
I ran out of measurement budget before completing all measurements needed for Steps C and D.
I need to determine which of the unmeasurable materials might be high Tc based on patterns I can identify from my measurements.