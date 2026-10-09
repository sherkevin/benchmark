# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90K, Low Tc: < 90K

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
Series members and Tc:
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**Peak T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (also O6.220 at 127.0)

## Step B: 4-element Hg materials (O-Cu-Ba-Hg) with Tc >= 95.0
threshold1 = 127 - 32 = 95.0 K

4-element Hg measurements:
| Composition | Tc (K) | Status | Q1? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO SAMPLE | — |
| O4.066Cu1Ba2Hg1 | — | NO SAMPLE | — |
| O4.081Cu1Ba2Hg1 | ? | not measured (budget) | ? |
| O4.085Cu1Ba2Hg1 | ? | not measured (budget) | ? |
| O4.096Cu1Ba2Hg1 | 95.6 | measured | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | measured | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | measured | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | measured | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | measured | ✓ |
| O4.160Cu1Ba2Hg1 | — | NO SAMPLE | — |
| O4.162Cu1Ba2Hg1 | 97.4 | measured | ✓ |
| O4.180Cu1Ba2Hg1 | — | NO SAMPLE | — |
| O4.200Cu1Ba2Hg1 | 94.2 | measured | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | measured | ✓ |

**Q1 = {O4.096, O4.097, O4.150, O4.162, O4.220}**

## Other materials found
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (LOW! has Tl, no Ca, 5 elements)
- O10Ca2Cu3Ba2Tl2: not measured (from Ca search, 5 elements)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: not measured (from Ca search, 6 elements)
- O8Ca0.850Cu2.200Ba2Tl1.800: not measured (from Ca search, 5 elements)

## Step C: Rule Induction
All measurable Hg materials are HIGH (>=90K) EXCEPT O6Cu1Ba2Hg1Tl1 (Tc=0, LOW).
Distinguishing feature of O6Cu1Ba2Hg1Tl1: 5 elements AND no Ca.

**Rule: HIGH if (exactly 4 elements) OR (contains Ca). LOW if (5+ elements AND no Ca).**

Confirmed unmeasurable materials and predictions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, has Ca) → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, has Ca) → HIGH
3. O4.047Cu1Ba2Hg1 (4 elem) → HIGH
4. O4.066Cu1Ba2Hg1 (4 elem) → HIGH
5. O4.160Cu1Ba2Hg1 (4 elem) → HIGH
6. O4.180Cu1Ba2Hg1 (4 elem) → HIGH

## Step D: Final Pick
Among HIGH unmeasurable, fewest elements = 4.
Four 4-element candidates: O4.047, O4.066, O4.160, O4.180.
Tie-break by higher expected Tc:
- O4.160: between O4.150 (97.0) and O4.162 (97.4) → expected ~97K (highest)
- O4.180: between O4.162 (97.4) and O4.200 (94.2) → expected ~96K
- O4.047, O4.066: low oxygen, expected lower Tc

**Final pick: O4.160Cu1Ba2Hg1**

## Budget Summary
- Searches: 14 (Ca: 5 pages, Hg: 9 pages) = 14 units
- Measurements: 20 = 20 units
- Total: 34/34
