# Notes - Superconductor Tc Study

## Budget: 34/34 used

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, 5 elements)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | HIGH |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | HIGH ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | HIGH |

**T* = 127.0 K (peak at O6.210Ca1Cu2Ba2Hg1)**

## 4-element Hg materials (O-Cu-Ba-Hg)
| Composition | Tc (K) | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.066Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.081Cu1Ba2Hg1 | 98.0 | HIGH |
| O4.085Cu1Ba2Hg1 | 92.0 | HIGH |
| O4.096Cu1Ba2Hg1 | 95.6 | HIGH |
| O4.097Cu1Ba2Hg1 | 95.3 | HIGH |
| O4.101Cu1Ba2Hg1 | 93.4 | HIGH |
| O4.103Cu1Ba2Hg1 | 92.5 | HIGH |
| O4.150Cu1Ba2Hg1 | 97.0 | HIGH |
| O4.160Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.162Cu1Ba2Hg1 | 97.4 | HIGH |
| O4.180Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.200Cu1Ba2Hg1 | 94.2 | HIGH |
| O4.220Cu1Ba2Hg1 | 97.0 | HIGH |

## Other Hg materials
| Composition | Tc (K) | Status |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW (has Tl) |

## Non-Hg material found via Cu search
| Composition | Tc (K) | Status |
|---|---|---|
| O10Ca2Cu3Ba2Tl2 | unknown | couldn't measure (budget) |

## Step B: threshold1 = 127.0 - 32 = 95.0
Q1 = Hg + 4 elements + Tc >= 95.0:
- O4.081Cu1Ba2Hg1: 98.0
- O4.096Cu1Ba2Hg1: 95.6
- O4.097Cu1Ba2Hg1: 95.3
- O4.150Cu1Ba2Hg1: 97.0
- O4.162Cu1Ba2Hg1: 97.4
- O4.220Cu1Ba2Hg1: 97.0

## Step C: Rule Induction
All measurable materials:
- 4-element (O,Cu,Ba,Hg): ALL HIGH (10/10)
- 5-element (O,Ca,Cu,Ba,Hg): ALL HIGH (10/10)
- 5-element with Tl (O,Cu,Ba,Hg,Tl): LOW (0 K)

**Rule: "Contains Hg and does NOT contain Tl → high; otherwise → low"**
(Multi-factor: considers both Hg and Tl presence)

## Unmeasurable Materials & Predictions
1. O6.160Ca1Cu2Ba2Hg1 (5 elem): has Hg, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem): has Hg, no Tl → HIGH
3. O4.047Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
4. O4.066Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
5. O4.160Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH
6. O4.180Cu1Ba2Hg1 (4 elem): has Hg, no Tl → HIGH

## Step D: Final Pick
Among unmeasurable "high", fewest elements:
- 4 elements: O4.047, O4.066, O4.160, O4.180 (all tied at 4)
- 5 elements: O6.160, O6.270

Break tie by higher expected Tc:
- O4.160: near O4.150(97.0) and O4.162(97.4) → ~97 K
- O4.180: near O4.162(97.4) and O4.200(94.2) → ~96 K
- O4.066: below O4.081(98.0) → ~94 K
- O4.047: even lower oxygen → ~92 K

**Final: O4.160Cu1Ba2Hg1** (highest expected Tc among 4-element unmeasurables)
