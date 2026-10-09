# Notes - Superconductor Tc Study

## Budget Usage
- 9 search_db calls (Hg pages 1-9): 9 units
- 9 successful measure_tc calls: 9 units  
- 2 failed measure_tc (no sample, free): 0 units
- Total: 18/18

## All 27 Hg-containing materials discovered

### 4-element series (O, Cu, Ba, Hg) - 14 materials:
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1,
O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1,
O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

### 5-element Ca-series (O, Ca, Cu, Ba, Hg) - 12 materials:
O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, 
O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1,
O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1,
O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

### 5-element Tl material (O, Cu, Ba, Hg, Tl) - 1 material:
O6Cu1Ba2Hg1Tl1

## Measurements

### Ca-series (Step A):
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured (seed) |
| O6.100Ca1Cu2Ba2Hg1 | ? | not checked (budget exhausted) |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | ? | not checked |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | ? | not checked |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

Tc dome: 92 → 102 → 122 → **127** → 123 → 100 → 98
Peak at O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K

### 4-element series:
| Composition | Tc (K) | Status |
|---|---|---|
| O4.101Cu1Ba2Hg1 | 93.4 | measured |
| All other 13 | ? | not checked |

### Tl material:
| Composition | Tc (K) | Status |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | measured |

## Step A: Peak
- Composition: O6.210Ca1Cu2Ba2Hg1
- T* = 127.0 K

## Step B: Filter family B
- threshold1 = 127 - 32 = 95
- 4-element Hg materials: O{var}Cu1Ba2Hg1 series
- Only measured O4.101Cu1Ba2Hg1 = 93.4 < 95
- Qualifying set Q1: likely empty based on available data

## Step C: Predictions for unmeasurable materials
Confirmed unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: In Ca-series, between O6.130 (102) and O6.190 (122). Interpolation: ~112 K → HIGH
- O6.270Ca1Cu2Ba2Hg1: In Ca-series, between O6.240 (123) and O6.300 (100). Interpolation: ~111.5 K → HIGH

Rule induced:
- Multi-factor: Materials with Ca AND 2Cu atoms in Hg-Ba-Cu-O system have high Tc (dome-shaped, all >= 90K)
- Tl substitution destroys superconductivity (Tc = 0)
- 4-element (no Ca) materials have moderate Tc (~93K, borderline high)
- Single-factor "5 elements → high" would fail for Tl material (5 elements but Tc=0)

## Step D: Final pick
- Both unmeasurable materials have 5 elements
- O6.160Ca1Cu2Ba2Hg1: expected Tc ~112 K
- O6.270Ca1Cu2Ba2Hg1: expected Tc ~111.5 K
- Tie-break: O6.160Ca1Cu2Ba2Hg1 (higher expected Tc)
