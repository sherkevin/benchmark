# Notes on Superconductor Tc Study - Final Analysis

## Step A — Series Peak (T*)
Homologous series: O,Ca,Cu,Ba,Hg (same as seed)

Measurements:
| Composition | Tc |
|-------------|-----|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B — Filter Family B
threshold1 = 127.0 - 32 = **95.0**

4-element Hg materials (O,Cu,Ba,Hg series):
| Composition | Tc | >= 95? |
|-------------|-----|--------|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | - |

**Q1 (qualifying): O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1**

Note: O4.066, O4.150, O4.160, O4.162, O4.200, O4.220 not measured due to budget.

## Step C — Predictions for Unmeasurable Materials

Found 3 unmeasurable materials:
1. **O6.160Ca1Cu2Ba2Hg1** (5 elements): Between O6.140 (105) and O6.190 (122). Interpolated ~112. **HIGH**
2. **O6.270Ca1Cu2Ba2Hg1** (5 elements): Between O6.240 (123) and O6.300 (100). Interpolated ~111.5. **HIGH**
3. **O4.180Cu1Ba2Hg1** (4 elements): O4 series shows declining trend after 4.096. At 4.180, likely below 90. **LOW**

## Step D — Final Pick
Among predicted HIGH unmeasurable:
- O6.160Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~112)
- O6.270Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~111.5)

Both have 5 elements (tie). Tiebreaker: higher expected Tc.
**Final: O6.160Ca1Cu2Ba2Hg1**

## Induced Rule
"Within each homologous series (same non-oxygen elements), Tc follows a dome-shaped curve with oxygen content, peaking at a series-specific optimal level. To predict Tc for unmeasurable compositions, interpolate between measured neighbors in the same series."
