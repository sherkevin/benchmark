# Superconductor Tc Study Notes

## Seed Material
- Composition: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements)
- Tc = 92.0 K

## Step A — Find series peak (homologous series with same elements, varying oxygen)
- Series: Ca1Cu2Ba2Hg with varying O stoichiometry
- 12 members found

### Measurements:
| Composition | Tc | Available |
|-------------|-----|-----------|
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
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first one achieving max)

## Step B — Filter family B (contains Hg, exactly 4 elements)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Family B materials (Hg + 4 elements): O4.xxxCu1Ba2Hg1 series (14 total)

### Measurements for Family B:
| Composition | Tc | Sample | >= 95.0? |
|-------------|-----|--------|----------|
| O4.047Cu1Ba2Hg1 | N/A | No | ? |
| O4.066Cu1Ba2Hg1 | N/A | No | ? |
| O4.081Cu1Ba2Hg1 | 98.0 | Yes | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | Yes | No |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | Yes | No |
| O4.103Cu1Ba2Hg1 | 92.5 | Yes | No |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes | YES |
| O4.160Cu1Ba2Hg1 | N/A | No | ? |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes | YES |
| O4.180Cu1Ba2Hg1 | N/A | No | ? |
| O4.200Cu1Ba2Hg1 | 94.2 | Yes | No |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes | YES |

**Q1 (measurable with Tc >= 95.0):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C — Extrapolate for unmeasurable materials
- Found unmeasurable: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1 (from series)
- Also: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1 (from family B)

**Induced Rule:** Materials with oxygen content within the measured range for their family are high (Tc>=90); those outside this range are low.
- 4-element family range: O4.081-4.220 (all measurable are high)
- 5-element family range: O6.080-6.310 (all measurable are high)

**Predictions:**
- O6.160Ca1Cu2Ba2Hg1: high (within 5-elem range)
- O6.270Ca1Cu2Ba2Hg1: high (within 5-elem range)
- O4.047Cu1Ba2Hg1: low (below 4-elem range)
- O4.066Cu1Ba2Hg1: low (below 4-elem range)
- O4.160Cu1Ba2Hg1: high (within 4-elem range)
- O4.180Cu1Ba2Hg1: high (within 4-elem range)

## Step D — Final pick
- Unmeasurable predicted high: O6.160 (5 elem), O6.270 (5 elem), O4.160 (4 elem), O4.180 (4 elem)
- Fewest elements: 4 (O4.160 and O4.180)
- Tie-break by higher expected Tc:
  - O4.160: ~97.2 (between 97.0 and 97.4)
  - O4.180: ~95.8 (between 97.4 and 94.2)
- **Final answer: O4.160Cu1Ba2Hg1**

## Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K
- Q1 qualifying: 6 materials
- Predictions: 6 unmeasurable materials (4 high, 2 low)
- Final: O4.160Cu1Ba2Hg1
