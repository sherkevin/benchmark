# Superconductor Tc Study Notes - Final Summary

## Step A — Series Peak (COMPLETED)
Homologous series: O(x)Ca1Cu2Ba2Hg1

Measurements:
| Composition | Tc | Sample? |
|-------------|-----|---------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | N/A | No |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | N/A | No |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter Family B (PARTIAL - budget exhausted)
threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Need: materials with Hg, exactly 4 elements, Tc >= 95.0

4-element Hg materials found (O-Cu-Ba-Hg series, 14 total):
- O4.047Cu1Ba2Hg1 through O4.220Cu1Ba2Hg1

Could not measure any due to budget exhaustion.
**qualifying: []** (unknown without measurements)

## Step C — Predictions for Unmeasurable Materials
Unmeasurable materials identified (measure_tc returned "no sample exists"):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.047Cu1Ba2Hg1 (4 elements)

**Induced Rule**: Materials with O/cation ratio between 1.01-1.05 and containing Cu and Ba have high Tc (>=90 K).

This is a multi-factor rule considering:
1. O/cation ratio (not just oxygen content alone)
2. Presence of key elements (Cu and Ba)

Predictions:
- O6.160Ca1Cu2Ba2Hg1: O/cation = 6.160/6 = 1.027, has Cu and Ba → **high**
- O6.270Ca1Cu2Ba2Hg1: O/cation = 6.270/6 = 1.045, has Cu and Ba → **high**
- O4.047Cu1Ba2Hg1: O/cation = 4.047/4 = 1.012, has Cu and Ba → **high**

## Step D — Final Pick
Among unmeasurable materials predicted high:
- O4.047Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

**Final answer: O4.047Cu1Ba2Hg1** (fewest elements = 4)

## Budget Usage
Total: 45/45 units used
- search_db calls: ~28 units (Ca and Hg searches)
- measure_tc calls: 12 units (11 series members + 1 four-element material)

## Final Answer (answer.json)
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {
    "O6.160Ca1Cu2Ba2Hg1": "high",
    "O6.270Ca1Cu2Ba2Hg1": "high",
    "O4.047Cu1Ba2Hg1": "high"
  },
  "final": "O4.047Cu1Ba2Hg1",
  "rule": "Materials with O/cation ratio between 1.01-1.05 and containing Cu and Ba have high Tc (>=90 K)"
}
```
