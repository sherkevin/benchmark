# Notes on Superconductor Tc Study

## Budget: 18 units total (EXHAUSTED)

## Step A: Series peak - COMPLETED
Homologous series: O_xCa1Cu2Ba2Hg1

Measured results:
| Composition | Tc (K) | Status |
|-------------|--------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter family B - INCOMPLETE
threshold1 = 127.0 - 32 = **95.0**

Criteria: Hg + exactly 4 elements + Tc >= 95.0

From Hg search page 1 (only page affordable):
- O4.047Cu1Ba2Hg1 (4 elements) - Tc unknown
- O4.066Cu1Ba2Hg1 (4 elements) - Tc unknown  
- O4.081Cu1Ba2Hg1 (4 elements) - Tc unknown

**qualifying = []** (cannot verify without measurements)

## Step C: Predictions for unmeasurable materials
Unmeasurable found:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)

**Rule**: Tc follows oxygen doping dome - interpolate between neighbors.

Predictions:
- O6.160: Between 105K (O6.140) and 122K (O6.190) → ~112K → **high**
- O6.270: Between 123K (O6.240) and 100K (O6.300) → ~111.5K → **high**

## Step D: Final pick
Both have 5 elements (tie).
O6.160 expected Tc (~112K) > O6.270 expected Tc (~111.5K)
**Final: O6.160Ca1Cu2Ba2Hg1**

## Answer Summary
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows oxygen doping dome pattern - interpolate between neighboring measured compositions"
}
```
