# Notes - Superconductor Tc Study

## Budget Tracking
- Total budget: 34 units
- Used: 34 (exhausted)
- Breakdown: 11 search_db pages + 12 series measurements + 3 four-element measurements + 8 Cu1Ba2Hg1 search pages = 34

## Seed Material
- O6.080Ca1Cu2Ba2Hg1 (contains O, Ca, Cu, Ba, Hg - 5 elements)

## Step A: Homologous Series (Ca1Cu2Ba2Hg1 with varying O)
Found 12 members via search "Ca1Cu2Ba2Hg1" (6 pages):

| Composition | Tc (K) | Sample? | High/Low |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes | high |
| O6.160Ca1Cu2Ba2Hg1 | N/A | NO SAMPLE | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes | high ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes | high ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes | high |
| O6.270Ca1Cu2Ba2Hg1 | N/A | NO SAMPLE | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes | high |

**Peak (T*): 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)
Tc dome shape: rises from 92→127, then falls back to 98

## Step B: 4-element Hg materials (O-Cu-Ba-Hg family)
threshold1 = T* - 32 = 127 - 32 = 95 K

Found 15 Cu1Ba2Hg1 materials (8 pages), 14 with exactly 4 elements:
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 Cu1Ba2Hg1
Plus O6Cu1Ba2Hg1Tl1 (5 elements - excluded)

Measured 3 of 14:
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (LOW, < 95)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (HIGH, ≥ 95) ✓ QUALIFYING
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (LOW, < 95)

Quadratic fit through 3 data points predicts dome peaking near O4.150 at ~97 K.
Materials near O4.150 likely qualify: O4.160 (~97.0), O4.162 (~96.9), O4.180 (~96.1)

Accounting for all 27 Hg materials: 12 (5-element series) + 14 (4-element) + 1 (O6Cu1Ba2Hg1Tl1) = 27 ✓

## Step C: Unmeasurable materials
Confirmed unmeasurable (tried measure_tc, got "no sample"):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) - between O6.140(105K) and O6.190(122K), expected ~113K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) - between O6.240(123K) and O6.300(100K), expected ~112K → HIGH

Rule: Dome-shaped Tc vs oxygen content. Materials near optimal oxygen doping are high (≥90K). 
For 5-element series, all measurable are high (92-127K). Both unmeasurable fall in the high-Tc dome region.
Multi-factor rule: High if (has Cu and Ba and O) AND (oxygen content in reasonable range for the structure type).

## Step D: Final pick
Among unmeasurable predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected ~113K
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected ~112K
Both have 5 elements (tied). O6.160 has slightly higher expected Tc → FINAL PICK

## Summary
- Peak: O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K
- Threshold: 95.0 K
- Q1: O4.150Cu1Ba2Hg1 (only confirmed measurement ≥ 95)
- Unmeasurable predictions: both high
- Final: O6.160Ca1Cu2Ba2Hg1
