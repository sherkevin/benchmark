# Notes on Superconductor Tc Study

## Seed Material
- Composition: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements)

## Step A — Series Peak (COMPLETED)

Homologous series members (same element set {O, Ca, Cu, Ba, Hg}):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* (peak Tc) = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

**threshold1 = T* - 32 = 127 - 32 = 95**

## Step B — Filter Family B (contains Hg, exactly 4 elements, Tc >= 95)

Hg materials found (pages 1-3 of 9 pages, 27 total):
- O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.081Cu1Ba2Hg1 (4 elements)
- O4.085Cu1Ba2Hg1 (4 elements)
- O4.096Cu1Ba2Hg1 (4 elements)
- O4.097Cu1Ba2Hg1 (4 elements)
- O4.101Cu1Ba2Hg1 (4 elements)
- O4.103Cu1Ba2Hg1 (4 elements)
- O4.150Cu1Ba2Hg1 (4 elements)

BUDGET EXHAUSTED before measuring these. Cannot determine which have Tc >= 95.
Assuming qualifying = [] (empty) due to lack of measurements.

## Step C — Predictions for unmeasurable materials

Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1: Expected Tc ~112 K (interpolating between O6.140@105 and O6.190@122) -> HIGH
- O6.270Ca1Cu2Ba2Hg1: Expected Tc ~111.5 K (interpolating between O6.240@123 and O6.300@100) -> HIGH

## Step D — Final pick

Both unmeasurable materials have 5 elements. Tie-breaker: higher expected Tc.
- O6.160Ca1Cu2Ba2Hg1: ~112 K
- O6.270Ca1Cu2Ba2Hg1: ~111.5 K

**Final answer: O6.160Ca1Cu2Ba2Hg1** (slightly higher expected Tc)

## Rule induced

"For materials in a homologous series (same element set), Tc follows a dome-shaped curve with respect to oxygen content. Tc increases with oxygen up to an optimal level, then decreases. For this Hg-Ba-Ca-Cu-O series, the peak is around O6.210-220 with Tc=127K. Materials with oxygen between O6.130 and O6.310 have Tc >= 90K (high). Unmeasurable compositions can be predicted by interpolating between neighboring measured compositions on the doping curve."

## Budget breakdown
- Hg search page 1: 1
- Ca search pages 1-5: 5
- Measurements (10 successful): 10
- Hg search pages 2-3: 2
- Total: 18/18 (exhausted)

Mistake: Should have reserved budget for measuring 4-element Hg materials for Step B.
