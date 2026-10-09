# Notes for Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- Definition: high Tc >= 90 K, low < 90 K

## Step A — Find peak Tc in homologous series
Measured compositions with same elements (O, Ca, Cu, Ba, Hg):

| Composition | Tc (K) |
|-------------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | no sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | no sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
**peak = O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B — Filter family B (contains Hg, exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = **95.0**

4-element Hg materials (O, Cu, Ba, Hg) measured:
| Composition | Tc (K) | >= 95.0? |
|-------------|--------|----------|
| O4.047Cu1Ba2Hg1 | 95.0 | Yes |
| O4.081Cu1Ba2Hg1 | 98.0 | Yes |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes |
| O4.200Cu1Ba2Hg1 | 94.2 | No |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes |
| O4.180Cu1Ba2Hg1 | no sample | - |
| O4.101Cu1Ba2Hg1 | not measured | - |
| O4.103Cu1Ba2Hg1 | not measured | - |
| O4.085Cu1Ba2Hg1 | not measured | - |

**Q1** = [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]

## Step C — Extrapolate rule
Induced rule: "Material is high if it contains Hg or Tl"

This rule accounts for:
- All Hg-containing materials being high (based on measurements)
- Tl-containing materials also being high (known high-Tc superconductors)
- Non-Hg, non-Tl materials being low

Predictions for unmeasurable.txt:
- All Hg-containing (lines 1-22): high
- Tl-containing without Hg (lines 23-24): high
- Non-Hg, non-Tl (lines 25-26): low

## Step D — Final pick
Among predicted high unmeasurable materials:
- 4-element: O4.xxxCu1Ba2Hg1 series (11 materials)
- 5-element: O6.xxxCa1Cu2Ba2Hg1 series, O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750

Fewest elements = 4 (O4.xxx series)

Among 4-element materials, tie-breaker is higher expected Tc:
- O4.081: 98.0 (measured, highest)
- O4.162: 97.4
- O4.150: 97.0
- O4.220: 97.0
- O4.096: 95.6
- O4.097: 95.3
- O4.047: 95.0
- O4.200: 94.2
- O4.101, O4.103, O4.085: not measured, estimated ~95-97

**Final answer: O4.081Cu1Ba2Hg1** (4 elements, highest measured Tc of 98.0)

## Budget Usage
- Total budget: 18
- Used: 18 (all on valid measurements)
- No-sample measurements (O6.160, O6.270, O4.180) did not consume budget
