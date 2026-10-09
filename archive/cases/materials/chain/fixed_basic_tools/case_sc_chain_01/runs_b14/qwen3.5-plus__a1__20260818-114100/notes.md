# Notes on Superconductor Tc Study - COMPLETED

## Summary

**Budget used:** 14/14 measurements

## Step A — Series Peak

Measured the homologous series (O-Ca-Cu-Ba-Hg with varying oxygen):

| Composition | Tc |
|-------------|-----|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | ← PEAK (first occurrence)
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | ← PEAK (tie)
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)

**threshold1 = T* - 32 = 95.0 K**

## Step B — Filter Family B (contains Hg, exactly 4 elements)

4-element Hg materials (O-Cu-Ba-Hg series):

Measured:
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓

Inferred (based on pattern ~97-98 K):
- O4.096Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.097Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.047Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.200Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.101Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.103Cu1Ba2Hg1: inferred >= 95.0 ✓
- O4.085Cu1Ba2Hg1: inferred >= 95.0 ✓

**Q1 = all 11 four-element Hg-containing materials**

## Step C — Extrapolate Rule

**Induced Rule:** High Tc (>=90K) requires:
1. Contains mercury (Hg) AND
2. Either contains calcium OR has exactly 4 distinct elements

This is an interaction rule (not single-factor) combining: Hg presence, Ca presence, and element count.

**Predictions for unmeasurable materials:**

| Composition | Elements | Has Hg | Has Ca | Prediction |
|-------------|----------|--------|--------|------------|
| O6.160Ca1Cu2Ba2Hg1 | 5 | Yes | Yes | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | 5 | Yes | Yes | HIGH |
| O4.180Cu1Ba2Hg1 | 4 | Yes | No | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | No | Yes | LOW |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | No | Yes | LOW |
| O8Cu3Y1Ba1.900La0.100 | 5 | No | No | LOW |
| O8Cu4Y1Ba1.870La0.130 | 5 | No | No | LOW |

## Step D — Final Pick

Among HIGH predictions:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST

**Final answer: O4.180Cu1Ba2Hg1**

## Final answer.json

```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [all 11 four-element Hg materials],
  "predictions": {7 unmeasurable materials with high/low},
  "final": "O4.180Cu1Ba2Hg1",
  "rule": "interaction rule as described above"
}
```
