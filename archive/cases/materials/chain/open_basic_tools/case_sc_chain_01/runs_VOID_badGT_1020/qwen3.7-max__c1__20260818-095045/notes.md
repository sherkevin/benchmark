# Notes - Superconductor Tc Study

## Budget: 34/34 used (10 search pages + 24 measurements; unmeasurable ones were free)

## All Materials Discovered

### Hg Search (27 materials, 9 pages)

**5-element O-Ca-Cu-Ba-Hg series (seed's homologous series):**
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable, HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measurable, HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measurable, HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measurable, HIGH |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable, HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measurable, HIGH ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measurable, HIGH ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable, HIGH |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measurable, HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measurable, HIGH |

**4-element O-Cu-Ba-Hg series:**
| Composition | Tc (K) | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.066Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.081Cu1Ba2Hg1 | 98.0 | measurable, HIGH |
| O4.085Cu1Ba2Hg1 | 92.0 | measurable, HIGH |
| O4.096Cu1Ba2Hg1 | 95.6 | measurable, HIGH |
| O4.097Cu1Ba2Hg1 | 95.3 | measurable, HIGH |
| O4.101Cu1Ba2Hg1 | 93.4 | measurable, HIGH |
| O4.103Cu1Ba2Hg1 | 92.5 | measurable, HIGH |
| O4.150Cu1Ba2Hg1 | 97.0 | measurable, HIGH |
| O4.160Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.162Cu1Ba2Hg1 | 97.4 | measurable, HIGH |
| O4.180Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.200Cu1Ba2Hg1 | 94.2 | measurable, HIGH |
| O4.220Cu1Ba2Hg1 | 97.0 | measurable, HIGH |

**Other Hg material (5 elements):**
| O6Cu1Ba2Hg1Tl1 | 0.0 | measurable, LOW |

### Tl Search (5 matches, saw 3 of 5 — page 1 only)
| O10Ca2Cu3Ba2Tl2 | — | measurable (couldn't measure, budget) |
| O6Cu1Ba2Hg1Tl1 | 0.0 | already known |
| O6Cu1Cd0.250Ba2Tl1.750 | — | measurable (couldn't measure, budget) |

### Y Search (3 matches)
| O7Cu3Y0.200Ba2La0.800 | — | measurable (couldn't measure, budget) |
| O8Cu3Y1Ba1.900La0.100 | — | UNMEASURABLE |
| O8Cu4Y1Ba1.870La0.130 | — | UNMEASURABLE |

### Bi Search: no results

## Step A: Series Peak
- T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (tie with O6.220Ca1Cu2Ba2Hg1)
- Picking O6.210Ca1Cu2Ba2Hg1 as the peak

## Step B: Q1 (4-elem Hg with Tc >= 95.0 K)
- threshold1 = 127.0 - 32 = 95.0 K
- Qualifying: O4.081Cu1Ba2Hg1 (98.0), O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3), O4.150Cu1Ba2Hg1 (97.0), O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0)

## Step C: Rule Induction
Only one LOW material: O6Cu1Ba2Hg1Tl1 (Tc=0.0 K)
- It has Tl and Hg but no Ca; Cu=1 with O=6 (excess O for single-layer structure)
- All HIGH materials either have Ca (5-elem series) or O<5 (4-elem series)

Multi-factor rule: "HIGH if contains Ca OR contains Y OR O<5 (consistent with structure type). LOW if lacks Ca and Y while having high oxygen (O≥5) incompatible with single-CuO2-layer structure."

A single-factor rule like "O>5 → LOW" would wrongly classify the Ca-containing 5-element series (O≈6 but HIGH) and Y-Ba-Cu-O (O≈8 but HIGH).
A single-factor rule like "has Tl → LOW" might be wrong for some Tl materials (e.g., O10Ca2Cu3Ba2Tl2 likely HIGH).

## Unmeasurable materials and predictions:
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (has Ca, between O6.140(105) and O6.190(122))
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (has Ca, between O6.240(123) and O6.300(100))
3. O4.047Cu1Ba2Hg1 → HIGH (O<5, consistent structure)
4. O4.066Cu1Ba2Hg1 → HIGH (O<5, consistent structure)
5. O4.160Cu1Ba2Hg1 → HIGH (O<5, interpolation ~97 K)
6. O4.180Cu1Ba2Hg1 → HIGH (O<5, interpolation ~96 K)
7. O8Cu3Y1Ba1.900La0.100 → HIGH (has Y, YBCO-type structure)
8. O8Cu4Y1Ba1.870La0.130 → HIGH (has Y, YBCO-type structure)

## Step D: Final Pick
Among HIGH unmeasurable, fewest elements = 4 elements:
- O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- Expected Tc: O4.160 ~97.2 (interpolated), O4.180 ~95.8, O4.066 ~97-98 (near peak at O4.081)
- Pick: O4.160Cu1Ba2Hg1 (reliable interpolation, ~97.2 K expected)
