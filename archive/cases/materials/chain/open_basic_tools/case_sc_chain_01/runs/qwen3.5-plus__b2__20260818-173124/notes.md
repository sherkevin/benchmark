# Superconductor Tc Study - Notes (FINAL)

## Budget: 26 units total (EXHAUSTED)

## Step A: Series Peak - COMPLETE
Seed: O6.080Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)

Homologous series members (O-Ca-Cu-Ba-Hg):
| Composition | Tc | Status |
|-------------|-----|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured <- PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured <- PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

**threshold1 = 127.0 - 32 = 95.0**

## Step B: Filter family B (Hg + exactly 4 elements, Tc >= 95.0)
4-element Hg materials identified (O-Cu-Ba-Hg series):
- O4.047Cu1Ba2Hg1 through O4.220Cu1Ba2Hg1 (14 materials)

PROBLEM: No budget left to measure these! 
**qualifying: []** (empty due to budget exhaustion)

## Step C: Unmeasurable materials found
- O6.160Ca1Cu2Ba2Hg1 (5 elements, no sample)
- O6.270Ca1Cu2Ba2Hg1 (5 elements, no sample)

Rule induced: Tc follows a dome-shaped relationship with oxygen content in the O-Ca-Cu-Ba-Hg series. 
- Peak at O6.210-6.220 (127 K)
- Rising slope: O6.080-6.210 (Tc increases from 92 to 127)
- Falling slope: O6.220-6.310 (Tc decreases from 127 to 98)

Predictions:
- O6.160Ca1Cu2Ba2Hg1: Between O6.140 (105) and O6.190 (122), expected ~113-115 K -> HIGH
- O6.270Ca1Cu2Ba2Hg1: Between O6.240 (123) and O6.300 (100), expected ~108-110 K -> HIGH

## Step D: Final pick
Among unmeasurable predicted "high":
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~113-115
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~108-110

Both have 5 elements (tie). Tie-break by higher expected Tc: **O6.160Ca1Cu2Ba2Hg1**

## Final Answer
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows a dome-shaped relationship with oxygen content: increases to a peak around O6.21-0.22, then decreases."
}
```

## Lessons Learned
- Budget management is critical. I spent too much on searching (13 units: 5 for Ca, 8 for Hg) instead of reserving for measurements.
- Optimal strategy would be: search minimally to identify targets, then measure strategically.
- Should have measured some 4-element Hg materials for Step B instead of exhaustively searching all pages.
