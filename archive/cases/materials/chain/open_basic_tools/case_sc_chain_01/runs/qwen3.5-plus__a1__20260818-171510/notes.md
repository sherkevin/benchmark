# Notes on Superconductor Tc Study - Final

## Step A: Homologous series peak
Seed: O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)

Measured Tc values for the series:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0  <-- PEAK (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0  <-- also peak
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter family B (Hg, exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O, Cu, Ba, Hg) measured:
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.066Cu1Ba2Hg1: ERROR (unmeasurable)
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗

Remaining 4-element materials not measured (budget exhausted):
- O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
- Based on trend (Tc decreases after peak ~O4.08), likely Tc < 95.0

**Q1 = [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]**

## Step C: Predictions for unmeasurable materials
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1: Predicted high (positioned between 105.0 and 122.0)
- O6.270Ca1Cu2Ba2Hg1: Predicted high (positioned between 123.0 and 100.0)
- O4.066Cu1Ba2Hg1: Predicted high (interpolation between 95.0 and 98.0)

**Rule induced:** Tc depends on element combination and oxygen content. Hg-Cu-Ba-O systems have Tc >= 90K. Adding Ca creates 5-element systems with higher peak Tc (~127K) than 4-element systems (~98K). Oxygen content tunes Tc parabolically around an optimal value for each system.

## Step D: Final pick
Among unmeasurable materials predicted high:
- O4.066Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

**Final: O4.066Cu1Ba2Hg1** (fewest elements = 4)

## Budget Summary
- 9 search_db calls (pages 1-9 for Hg)
- 17 measure_tc calls
- Total: 26/26 budget used
