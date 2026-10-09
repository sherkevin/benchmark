# Superconductor Tc Study Notes

## Step A - Finding peak Tc in homologous series

Seed: O6.080Ca1Cu2Ba2Hg1
Homologous series: O6.xxxCa1Cu2Ba2Hg1 (same elements, varying oxygen)

### Measurements of homologous series:
| Composition | Tc (K) |
|-------------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

Budget used: 10/18 for Step A

## Step B - Filter family B (contains Hg, exactly 4 elements)

threshold1 = T* - 32 = 127 - 32 = 95

### Measurements of 4-element Hg materials:
| Composition | Tc (K) | >= 95? |
|-------------|--------|--------|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO (but still HIGH by definition since 94.2 >= 90) |

Q1 (measured, Tc >= 95): O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Budget exhausted at 18 measurements.

## Step C - Induced Rule

From measurements:
- All 5-element Hg materials (O-Ca-Cu-Ba-Hg): Tc 92-127 K (all HIGH)
- All 4-element Hg materials (O-Cu-Ba-Hg): Tc 94.2-98 K (all HIGH, >= 90)

**Induced Rule: HIGH if (contains Hg with 4-5 elements) OR (contains Tl with exactly 5 elements) OR (contains both Y and La); LOW otherwise**

This is a multi-factor rule combining element presence AND element count.

### Predictions for unmeasurable materials:
| Composition | Elements | Prediction | Reason |
|-------------|----------|------------|--------|
| O6.160Ca1Cu2Ba2Hg1 | 5 | HIGH | Hg + 5 elements |
| O6.270Ca1Cu2Ba2Hg1 | 5 | HIGH | Hg + 5 elements |
| O4.180Cu1Ba2Hg1 | 4 | HIGH | Hg + 4 elements |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | HIGH | Tl + 5 elements |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | LOW | 6 elements, no matching pattern |
| O8Cu3Y1Ba1.900La0.100 | 5 | HIGH | Y + La combination |
| O8Cu4Y1Ba1.870La0.130 | 5 | HIGH | Y + La combination |

## Step D - Final Pick

Among HIGH predictions, find the one with FEWEST elements:
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- All others: 5 or 6 elements

**Final Answer: O4.180Cu1Ba2Hg1**
