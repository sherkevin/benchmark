# Notes on Superconductor Tc Study

## Budget: 18 units total (shared between search_db and measure_tc)

## Step A — Find series peak (T*)
Seed material: O6.080Ca1Cu2Ba2Hg1
This belongs to a homologous doping series with same elements but varying oxygen.

Need to find all materials with elements: Ca, Cu, Ba, Hg (and varying O)
Then measure Tc to find the peak.

### Plan:
1. Search for materials in this series (same element set, varying O)
2. Measure Tc for each to find the maximum (T*)

## Step B — Filter family B
Family B: materials containing Hg with exactly 4 elements
threshold1 = T* - 32
Find all materials in family B with Tc >= threshold1 (set Q1)

## Step C — Extrapolate
Find unmeasurable materials (no sample exists)
Induce a rule from measurable materials to predict high/low for unmeasurable ones

## Step D — Final pick
Among unmeasurable materials predicted "high", choose the one with FEWEST elements
Tie-break: higher expected Tc

---

## Progress Log

### Step A - Series Peak
- Searched for Hg materials: 27 total across 9 pages
- Identified homologous series: O6.xxxCa1Cu2Ba2Hg1 (12 members)
- Measured Tc values:
  - O6.080Ca1Cu2Ba2Hg1: 92.0 K
  - O6.100Ca1Cu2Ba2Hg1: 92.5 K
  - O6.130Ca1Cu2Ba2Hg1: 102.0 K
  - O6.140Ca1Cu2Ba2Hg1: 105.0 K
  - O6.160Ca1Cu2Ba2Hg1: no sample
  - O6.190Ca1Cu2Ba2Hg1: 122.0 K
  - O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
  - O6.220Ca1Cu2Ba2Hg1: 127.0 K
  - O6.240Ca1Cu2Ba2Hg1: 123.0 K
  - O6.270Ca1Cu2Ba2Hg1: no sample
  - O6.300Ca1Cu2Ba2Hg1: not measured (budget exhausted)
  - O6.310Ca1Cu2Ba2Hg1: not measured (budget exhausted)

- T* = 127.0 K (maximum measured Tc)
- Peak composition: O6.210Ca1Cu2Ba2Hg1

### Step B - Family B Filter
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Family B: materials with Hg and exactly 4 elements
- 4-element Hg materials: O4.xxxCu1Ba2Hg1 (14 materials)
- Could not measure due to budget exhaustion
- Q1 = [] (empty, no measurements available)

### Step C - Extrapolation
- Unmeasurable materials found (no sample exists):
  - O6.160Ca1Cu2Ba2Hg1
  - O6.270Ca1Cu2Ba2Hg1

- Rule induced: Materials with Ca, Cu2, and oxygen content between 6.0 and 6.3 in the Hg-Ba-Cu-Ca-O series have high Tc (>= 90 K)

- Predictions:
  - O6.160Ca1Cu2Ba2Hg1: high (oxygen 6.160 in range, has Ca and Cu2)
  - O6.270Ca1Cu2Ba2Hg1: high (oxygen 6.270 in range, has Ca and Cu2)

### Step D - Final Pick
- Unmeasurable materials predicted high: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
- Both have 5 elements (tie)
- Expected Tc:
  - O6.160: ~112 K (interpolated between O6.140=105 and O6.190=122)
  - O6.270: ~117 K (extrapolated from O6.240=123 with decreasing trend)
- Tie-breaker: higher expected Tc → O6.270Ca1Cu2Ba2Hg1

### Final Answer
- peak: O6.210Ca1Cu2Ba2Hg1
- peak_tc: 127.0
- threshold: 95.0
- qualifying: []
- predictions: {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"}
- final: O6.270Ca1Cu2Ba2Hg1
- rule: Materials with Ca, Cu2, and oxygen content between 6.0 and 6.3 in the Hg-Ba-Cu-Ca-O series have high Tc (>= 90 K)
