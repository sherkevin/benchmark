# Notes - Superconductor Tc Study

## Budget: 34 units (fully used)
- 7 Hg search pages (27 materials)
- 6 Cu search pages (35 materials, found O10Ca2Cu3Ba2Tl2)
- 12 homologous series measurements (2 unmeasurable, free)
- 14 four-element Hg measurements (4 unmeasurable, free)
- 1 Tl-material measurement

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | seed |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | ★ PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | ★ PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | |

**T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1**

## Step B: threshold1 = 127 - 32 = 95
4-element Hg materials (O-Cu-Ba-Hg):
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.066Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ Q1 |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ Q1 |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ Q1 |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ Q1 |
| O4.160Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ Q1 |
| O4.180Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ Q1 |

**Q1 = 6 materials**

## Other materials
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (LOW!) — has Tl, no Ca
- O10Ca2Cu3Ba2Tl2: found in Cu search, has Tl + Ca, not measured

## Step C: Rule Induction

### All measurable data:
- 20 HIGH: all have Hg, none have Tl. Tc range 92.0–127.0 K
- 1 LOW: O6Cu1Ba2Hg1Tl1 (Tc=0) — has Tl, no Ca

### Multi-factor rule:
- **HIGH** if: (Hg present AND Tl absent AND oxygen in measurable range) OR (Tl present AND Ca present)
- **LOW** if: (Tl present AND Ca absent) OR (oxygen below measurable range AND Ca absent)

### Why single-factor rules fail on "several":
- "no Tl → HIGH": wrong on O4.047, O4.066 (no Tl but LOW due to low oxygen) = 2 errors
- "Tl → LOW": wrong on O10Ca2Cu3Ba2Tl2 (has Tl but HIGH due to Ca) = 1 error
- Combined: single-factor Tl rule has 3 errors total = "several" ✓

### Predictions for 7 unmeasurable materials:
| Composition | Elements | Prediction | Reasoning |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4 | LOW | O=4.047, below measurable range (min O4.081), no Ca |
| O4.066Cu1Ba2Hg1 | 4 | LOW | O=4.066, below measurable range, no Ca |
| O4.160Cu1Ba2Hg1 | 4 | HIGH | Hg, no Tl, O in range (near O4.162=97.4) |
| O4.180Cu1Ba2Hg1 | 4 | HIGH | Hg, no Tl, O in range |
| O6.160Ca1Cu2Ba2Hg1 | 5 | HIGH | Hg, no Tl, has Ca |
| O6.270Ca1Cu2Ba2Hg1 | 5 | HIGH | Hg, no Tl, has Ca |
| O10Ca2Cu3Ba2Tl2 | 5 | HIGH | Tl + Ca |

## Step D: Final Pick
Among HIGH predicted unmeasurable materials:
- 4 elements: O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O10Ca2Cu3Ba2Tl2

Fewest elements = 4. Tie between O4.160 and O4.180.
Expected Tc: O4.160 ≈ 97 K (adjacent to O4.162=97.4), O4.180 ≈ 96 K (between 97.4 and 94.2).

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ≈ 97 K)
