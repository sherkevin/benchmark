# Notes on Superconductor Tc Study

## Budget: 26 units total (EXHAUSTED)

## Seed material: O6.080Ca1Cu2Ba2Hg1

## Step A - Series peak (DONE)
Homologous series: O(x)Ca1Cu2Ba2Hg1 (same elements, varying oxygen)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0  <-- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0  <-- PEAK
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)

## Step B - Filter family B (PARTIAL - budget exhausted)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓ QUALIFIES
- O4.066Cu1Ba2Hg1: NOT MEASURED
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓ QUALIFIES
- O4.085Cu1Ba2Hg1: NOT MEASURED
- O4.096Cu1Ba2Hg1: NOT MEASURED
- O4.097Cu1Ba2Hg1: NOT MEASURED
- O4.101Cu1Ba2Hg1: NOT MEASURED
- O4.103Cu1Ba2Hg1: NOT MEASURED
- O4.150Cu1Ba2Hg1: NOT MEASURED
- O4.160Cu1Ba2Hg1: NOT MEASURED
- O4.162Cu1Ba2Hg1: NOT MEASURED
- O4.180Cu1Ba2Hg1: NOT MEASURED
- O4.200Cu1Ba2Hg1: NOT MEASURED
- O4.220Cu1Ba2Hg1: NOT MEASURED

Q1 (measured): ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1"]

## Step C - Extrapolate
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1: Predicted HIGH (between 105.0 and 122.0)
- O6.270Ca1Cu2Ba2Hg1: Predicted HIGH (between 123.0 and 100.0)

## Step D - Final pick
Both unmeasurable have 5 elements (tie).
O6.160 expected Tc ~113-114
O6.270 expected Tc ~111-112
Final: O6.160Ca1Cu2Ba2Hg1 (higher expected Tc)

## Final Answer
- peak: O6.210Ca1Cu2Ba2Hg1
- peak_tc: 127.0
- threshold: 95.0
- qualifying: ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1"]
- predictions: {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"}
- final: O6.160Ca1Cu2Ba2Hg1
- rule: Tc follows a dome-shaped curve with oxygen content in homologous series
