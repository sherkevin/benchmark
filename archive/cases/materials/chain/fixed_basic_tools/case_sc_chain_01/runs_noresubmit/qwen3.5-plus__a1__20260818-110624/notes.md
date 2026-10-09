# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget
- 18 `measure_tc` calls maximum (all used)

## Important Discovery
The file names are counterintuitive:
- **unmeasurable.txt** actually contains materials WITH samples (can measure)
- **measurable.txt** contains materials WITHOUT samples (cannot measure)

## Step A — Series Peak
The seed belongs to homologous series: O-Ca-Cu-Ba-Hg (5 elements, varying oxygen).

### Series Measurements (from unmeasurable.txt):
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

**T* = 127.0** (maximum Tc)
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B — Filter Family B
- threshold1 = T* - 32 = 127.0 - 32 = **95.0**
- Materials with Hg AND exactly 4 elements: O4.xxxCu1Ba2Hg1 series

### Q1 Measurements:
| Composition | Tc (K) | >= 95.0? |
|-------------|--------|----------|
| O4.081Cu1Ba2Hg1 | 98.0 | Yes |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes |
| O4.047Cu1Ba2Hg1 | 95.0 | Yes |
| O4.200Cu1Ba2Hg1 | 94.2 | No |
| O4.101Cu1Ba2Hg1 | not measured | likely Yes |
| O4.103Cu1Ba2Hg1 | not measured | likely Yes |
| O4.085Cu1Ba2Hg1 | not measured | likely Yes |

**Q1** = All O4.xxxCu1Ba2Hg1 except O4.200Cu1Ba2Hg1

## Step C — Extrapolate Rule
### Induced Rule:
**A material has high Tc (>= 90 K) if and only if it contains BOTH Hg AND Cu.**

This is a multi-factor interaction rule: neither "contains Hg" alone nor "has Cu" alone guarantees high Tc, but the combination does.

### Predictions for measurable.txt materials:
| Composition | Elements | Has Hg? | Has Cu? | Prediction |
|-------------|----------|---------|---------|------------|
| O6.160Ca1Cu2Ba2Hg1 | 5 | Yes | Yes | high |
| O6.270Ca1Cu2Ba2Hg1 | 5 | Yes | Yes | high |
| O4.180Cu1Ba2Hg1 | 4 | Yes | Yes | high |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | No | Yes | low |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | No | Yes | low |
| O8Cu3Y1Ba1.900La0.100 | 5 | No | Yes | low |
| O8Cu4Y1Ba1.870La0.130 | 5 | No | Yes | low |

## Step D — Final Pick
Among materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← **FEWEST elements**

**Final Answer: O4.180Cu1Ba2Hg1**

## Budget Usage
- Step A: 10 measurements (O6.xxx series)
- Step B: 8 measurements (O4.xxx series, 3 left unmeasured due to budget)
- Total: 18/18 used
