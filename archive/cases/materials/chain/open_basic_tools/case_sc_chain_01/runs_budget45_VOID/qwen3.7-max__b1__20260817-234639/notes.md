# Notes - Superconductor Tc Study

## Budget Tracking
- Total: 45 units
- Used: 45 (14 measurements + 17 search pages + 14 measurements = 45)
  - 1 seed measurement
  - 8 Ca search pages
  - 14 Hg search pages (all 27 results found)
  - 10 series measurements (excluding seed)
  - 14 four-element Hg material measurements (2 failed - no sample)
  - 4 other material measurements (2 failed - no sample)

## Step A: Homologous Series Peak
Series: O(x)Ca1Cu2Ba2Hg1 (same elements, varying oxygen)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

Peak: O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K

## Step B: Q1 (Hg + 4 elements + Tc >= 95.0 K)
threshold1 = 127.0 - 32 = 95.0 K

4-element Hg materials measured:
| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.066Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1

Other materials measured:
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (5 elem, LOW)
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (5 elem, HIGH)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: NO SAMPLE (6 elem)
- O8Ca0.850Cu2.200Ba2Tl1.800: NO SAMPLE (5 elem)

## Step C: Rule Induction

Measurable materials: 21 HIGH, 1 LOW
LOW material: O6Cu1Ba2Hg1Tl1 (Tc=0.0 K)
- Unique feature: only material with BOTH Hg AND Tl, without Ca
- Compare: O10Ca2Cu3Ba2Tl2 has Tl but also Ca → HIGH
- Compare: 4-element series has Hg but no Tl → HIGH

Rule: **LOW if (Hg present) AND (Tl present) AND (Ca absent); otherwise HIGH**

Single-factor rules that fail:
- "Has Tl → LOW" fails on O10Ca2Cu3Ba2Tl2 (HIGH) and the Tl-containing unmeasurable materials
- "Has Hg → HIGH" fails on O6Cu1Ba2Hg1Tl1 (LOW)

## Unmeasurable Predictions
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (no Tl, has Ca)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (no Tl, has Ca)
3. O4.047Cu1Ba2Hg1 → HIGH (no Tl, no Hg+Tl combo)
4. O4.066Cu1Ba2Hg1 → HIGH (no Tl)
5. O4.160Cu1Ba2Hg1 → HIGH (no Tl)
6. O4.180Cu1Ba2Hg1 → HIGH (no Tl)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 → HIGH (has Tl but has Ca, no Hg)
8. O8Ca0.850Cu2.200Ba2Tl1.800 → HIGH (has Tl but has Ca, no Hg)

## Step D: Final Pick
All 8 predicted HIGH. Fewest elements = 4:
- O4.047Cu1Ba2Hg1 (expected Tc ~90-95 K, below measurable range)
- O4.066Cu1Ba2Hg1 (expected Tc ~92-96 K)
- O4.160Cu1Ba2Hg1 (expected Tc ~97 K, near O4.150=97.0 and O4.162=97.4)
- O4.180Cu1Ba2Hg1 (expected Tc ~95-96 K, between O4.162=97.4 and O4.200=94.2)

Highest expected Tc: O4.160Cu1Ba2Hg1 (~97 K)

**Final pick: O4.160Cu1Ba2Hg1**
