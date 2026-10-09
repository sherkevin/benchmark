# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Step A - Series Peak (DONE)
Homologous series: OxCa1Cu2Ba2Hg1 (same element set: O, Ca, Cu, Ba, Hg)

### Measurements:
| Composition | Tc | Sample |
|-------------|-----|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | - | no sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | - | no sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**T* (peak Tc) = 127.0 K**
Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B - Filter Family B (DONE)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0
- Criteria: contains Hg AND exactly 4 elements
- 4-element Hg materials: OxCu1Ba2Hg1 series (14 total)

### Measurements of 4-element Hg materials:
| Composition | Tc | >= 95? |
|-------------|-----|--------|
| O4.047Cu1Ba2Hg1 | no sample | - |
| O4.066Cu1Ba2Hg1 | no sample | - |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | no sample | - |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | no sample | - |
| O4.200Cu1Ba2Hg1 | not measured | - |
| O4.220Cu1Ba2Hg1 | not measured | - |

**Q1** = ["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1"]

## Step C - Extrapolation (DONE)
Unmeasurable materials (no sample exists):
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

**Rule induced**: Hg-Ba-Cu-O based superconductors maintain high Tc (>= 90K) across a wide range of oxygen contents. Unmeasurable materials fall within composition ranges where measured neighbors all have high Tc, so they are predicted high.

**Predictions**: All unmeasurable materials predicted as "high"

## Step D - Final Pick (DONE)
Among unmeasurable predicted high:
- 4-element: O4.047, O4.066, O4.160, O4.180
- 5-element: O6.160, O6.270

Fewest elements = 4. Among 4-element ones:
- O4.160 has highest expected Tc (~97.2, interpolated between O4.150=97.0 and O4.162=97.4)

**Final answer**: O4.160Cu1Ba2Hg1

## Budget
- Total: 34
- Used: 34 (exhausted)
