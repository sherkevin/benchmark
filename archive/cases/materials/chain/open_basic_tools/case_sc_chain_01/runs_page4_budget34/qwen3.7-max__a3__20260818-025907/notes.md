# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90K, Low = Tc < 90K

## Budget Tracking
- Used: 34/34 (7 search + 27 measure attempts)
- Remaining: 0

## Step A: Homologous Series (O,Ca,Cu,Ba,Hg - varying oxygen)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measurable |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measurable |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measurable |
| O6.160Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measurable |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measurable |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable |
| O6.270Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measurable |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measurable |

**Peak T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (tied with O6.220)
Trend: rises from 92 to 127 then falls back to 98 — classic superconducting dome.

## Step B: 4-element Hg materials (O,Cu,Ba,Hg)
threshold1 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | ??? | NO SAMPLE |
| O4.066Cu1Ba2Hg1 | ??? | NO SAMPLE |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | ??? | NO SAMPLE |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | ??? | NO SAMPLE |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1**

## Other materials measured
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K → LOW (5 elements: O,Cu,Ba,Hg,Tl)

## All discovered materials (31 total from searches)
From Hg search (27): all listed above
From Tl search (5): O10Ca2Cu3Ba2Tl2, O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800
(4 new non-Hg materials not yet measured — budget exhausted)

## Step C: Rule Induction
**Measurable data summary:**
- All 4-element (O,Cu,Ba,Hg): HIGH (92-98 K)
- All 5-element with Ca (O,Ca,Cu,Ba,Hg): HIGH (92-127 K)  
- 5-element without Ca (O,Cu,Ba,Hg,Tl): LOW (0 K)

**Key observation:** The only LOW material lacks Ca and has 5+ elements.
Single-factor "has Tl → low" would be wrong on Tl materials that have Ca.
Single-factor "has Ca → high" would be wrong on 4-element materials (no Ca but high).

**Multi-factor rule: "Materials containing Ca OR having exactly 4 elements → HIGH; materials with 5+ elements lacking Ca → LOW"**

**Unmeasurable confirmed (6):**
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (has Ca)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (has Ca)
3. O4.047Cu1Ba2Hg1 → HIGH (4 elements)
4. O4.066Cu1Ba2Hg1 → HIGH (4 elements)
5. O4.160Cu1Ba2Hg1 → HIGH (4 elements)
6. O4.180Cu1Ba2Hg1 → HIGH (4 elements)

**Untested new materials (4, including in predictions):**
7. O10Ca2Cu3Ba2Tl2 (5 elem, has Ca) → HIGH
8. O6Cu1Cd0.250Ba2Tl1.750 (5 elem, no Ca) → LOW
9. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, has Ca) → HIGH
10. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, has Ca) → HIGH

## Step D: Final Pick
Among predicted HIGH, fewest elements:
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1

Tie-break by expected Tc (interpolation):
- O4.047: extrapolate below O4.081(98) → ~91
- O4.066: between O4.047 and O4.081 → ~94
- O4.160: between O4.150(97.0) and O4.162(97.4) → ~97.3
- O4.180: between O4.162(97.4) and O4.200(94.2) → ~95.9

**Final pick: O4.160Cu1Ba2Hg1** (highest expected Tc ~97.3 among 4-element)
