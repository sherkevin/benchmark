# Notes: Superconductor Tc Study

## Task Summary
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 14 measure_tc calls
- Step A: Find max Tc in homologous series (varying oxygen)
- Step B: Filter family B (contains Hg, exactly 4 elements), Tc >= T* - 32
- Step C: Predict high/low for unmeasurable materials (induce rule)
- Step D: Pick unmeasurable high with fewest elements

## Step A - Measuring the series

Series members (O{6.xxx}Ca1Cu2Ba2Hg1):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (first occurrence)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**peak = O6.210Ca1Cu2Ba2Hg1** (first occurrence)
**threshold1 = 127.0 - 32 = 95.0 K**

Measurements used: 10

## Step B - Family B (contains Hg, exactly 4 elements)

Family B = materials with Hg AND exactly 4 elements (O, Cu, Ba, Hg)

Measured:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 ✓
- O4.180Cu1Ba2Hg1: no sample (in unmeasurable.txt)

Pattern: All family B materials have Tc ~97-98 K, well above threshold.

Inferred qualifying (all have samples per measurable.txt):
- O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.200Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1

Total: 11 qualifying materials (excluding O4.180 which has no sample)

Measurements used: 4 (total: 14/14 - budget exhausted)

## Step C - Predictions for unmeasurable materials

Unmeasurable materials (7 total):
1. O6.160Ca1Cu2Ba2Hg1 - Has Hg, Cu, Ba → **high**
2. O6.270Ca1Cu2Ba2Hg1 - Has Hg, Cu, Ba → **high**
3. O4.180Cu1Ba2Hg1 - Has Hg, Cu, Ba → **high**
4. O8Ca0.850Cu2.200Ba2Tl1.800 - No Hg → **low**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - No Hg, no Ba → **low**
6. O8Cu3Y1Ba1.900La0.100 - No Hg → **low**
7. O8Cu4Y1Ba1.870La0.130 - No Hg → **low**

**Induced Rule:** Materials containing Hg, Cu, and Ba together have high Tc (>=90K).
Materials lacking any of these three elements have low Tc.

This rule is consistent with all measured data:
- All Step A series (Hg+Cu+Ba+Ca+O): 92-127 K, all high
- All Family B (Hg+Cu+Ba+O): 97-98 K, all high

The rule is an interaction of multiple factors (Hg AND Cu AND Ba), not a single factor.

## Step D - Final pick

Among predicted high unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, predicted high)

## Final Answer (answer.json)
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.047Cu1Ba2Hg1", "O4.200Cu1Ba2Hg1", "O4.101Cu1Ba2Hg1", "O4.103Cu1Ba2Hg1", "O4.085Cu1Ba2Hg1"],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high", "O4.180Cu1Ba2Hg1": "high", "O8Ca0.850Cu2.200Ba2Tl1.800": "low", "O7Ca1Cu2Sr2Tl0.500Pb0.500": "low", "O8Cu3Y1Ba1.900La0.100": "low", "O8Cu4Y1Ba1.870La0.130": "low"},
  "final": "O4.180Cu1Ba2Hg1",
  "rule": "Materials containing Hg, Cu, and Ba together have high Tc (>=90K); materials lacking any of these three elements have low Tc"
}
```
