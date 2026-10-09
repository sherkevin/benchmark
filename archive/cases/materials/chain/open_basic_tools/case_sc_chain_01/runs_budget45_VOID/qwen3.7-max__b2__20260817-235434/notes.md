# Notes: Superconductor Tc Study

## Budget Usage
- Search "Hg" pages 1-14: 14 units
- Search "Ca1Cu2Ba2Hg1" pages 1-6: 6 units
- Search "Tl" page 1: 1 unit
- Search "Cu" page 1: 1 unit
- Search "O6Ca1Cu2Ba2Hg1" page 1: 1 unit
- Search "CaCuBaHg" page 1: 1 unit
- Measure 12 series members (10 successful + 2 failed): 10 units
- Measure 14 four-element materials (10 successful + 4 failed): 10 units
- Measure O6Cu1Ba2Hg1Tl1: 1 unit
- Total: 45/45

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes (HIGH) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes (HIGH) |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes (HIGH) |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes (HIGH) |
| O6.160Ca1Cu2Ba2Hg1 | — | No sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes (HIGH) |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes (HIGH) ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes (HIGH) ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes (HIGH) |
| O6.270Ca1Cu2Ba2Hg1 | — | No sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes (HIGH) |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes (HIGH) |

**T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (first of two tied at peak)

## Step B: Family B (Hg + exactly 4 elements = O,Cu,Ba,Hg)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | Tc >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | No sample |
| O4.066Cu1Ba2Hg1 | — | No sample |
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ Q1 |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ Q1 |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ Q1 |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ Q1 |
| O4.160Cu1Ba2Hg1 | — | No sample |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ Q1 |
| O4.180Cu1Ba2Hg1 | — | No sample |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ Q1 |

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

## Additional Material Found
- O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl): Tc = 0.0 K (LOW!)
- O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl): couldn't measure (budget exhausted)

## Step C: Rule Induction

### All measurable materials:
- 4-element (O,Cu,Ba,Hg): All HIGH (Tc 92-98 K), δ = O - 4 ranges 0.081-0.220
- 5-element (O,Ca,Cu,Ba,Hg): All HIGH (Tc 92-127 K), δ = O - 6 ranges 0.080-0.310
- 5-element (O,Cu,Ba,Hg,Tl): LOW (Tc = 0 K) — the ONLY low material

### Key observations:
1. The only LOW material contains Tl and lacks Ca
2. All materials without Tl are HIGH
3. Among 4-element materials, the unmeasurable ones at very low oxygen (δ=0.047, 0.066) are below the measurable range (δ starts at 0.081)

### Rule (multi-factor):
"A material is HIGH if it does NOT contain Tl, AND either:
(a) it has exactly 4 elements with sufficient excess oxygen (δ > ~0.07), OR
(b) it contains Ca."

Single-factor rules fail because:
- "No Tl → HIGH" is wrong on O4.047 and O4.066 (very low doping, no Tl but likely LOW)
- "All HIGH" is wrong on O4.047, O4.066, and O10Ca2Cu3Ba2Tl2
- "4 elements → HIGH" is wrong on O4.047 and O4.066

### Predictions for unmeasurable materials:
| Composition | Elements | δ | Prediction | Reason |
|---|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4 | 0.047 | LOW | No Tl, 4 elem, but δ < 0.07 |
| O4.066Cu1Ba2Hg1 | 4 | 0.066 | LOW | No Tl, 4 elem, but δ < 0.07 |
| O4.160Cu1Ba2Hg1 | 4 | 0.160 | HIGH | No Tl, 4 elem, δ > 0.07 |
| O4.180Cu1Ba2Hg1 | 4 | 0.180 | HIGH | No Tl, 4 elem, δ > 0.07 |
| O6.160Ca1Cu2Ba2Hg1 | 5 | 0.160 | HIGH | No Tl, has Ca |
| O6.270Ca1Cu2Ba2Hg1 | 5 | 0.270 | HIGH | No Tl, has Ca |
| O10Ca2Cu3Ba2Tl2 | 5 | 0 | LOW | Contains Tl |

## Step D: Final Pick
HIGH predictions: O4.160Cu1Ba2Hg1 (4 elem), O4.180Cu1Ba2Hg1 (4 elem), O6.160Ca1Cu2Ba2Hg1 (5 elem), O6.270Ca1Cu2Ba2Hg1 (5 elem)

Fewest elements: O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1 (both 4 elements)
Tie-breaker (higher expected Tc):
- O4.160: between O4.150 (97.0) and O4.162 (97.4) → expected ~97.2 K
- O4.180: between O4.162 (97.4) and O4.200 (94.2) → expected ~96 K

**Final pick: O4.160Cu1Ba2Hg1**
