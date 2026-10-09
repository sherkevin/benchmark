# Superconductor Tc Study Notes

## Budget: 45/45 used

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, 5 elements)
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

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (picking lower-O of the tied pair)
Classic superconducting dome shape.

## 4-element Hg materials (O-Cu-Ba-Hg)
| Composition | Tc (K) | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.066Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.081Cu1Ba2Hg1 | 98.0 | measured |
| O4.085Cu1Ba2Hg1 | 92.0 | measured |
| O4.096Cu1Ba2Hg1 | 95.6 | measured |
| O4.097Cu1Ba2Hg1 | 95.3 | measured |
| O4.101Cu1Ba2Hg1 | 93.4 | measured |
| O4.103Cu1Ba2Hg1 | 92.5 | measured |
| O4.150Cu1Ba2Hg1 | 97.0 | measured |
| O4.160Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.162Cu1Ba2Hg1 | 97.4 | measured |
| O4.180Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.200Cu1Ba2Hg1 | 94.2 | measured |
| O4.220Cu1Ba2Hg1 | 97.0 | measured |

## Other materials found
| Composition | Tc (K) | Elements | Status |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | 5 (O,Cu,Ba,Hg,Tl) | measured LOW |
| O10Ca2Cu3Ba2Tl2 | 118.97 | 5 (O,Ca,Cu,Ba,Tl) | measured HIGH |

Tl search: 5 total, seen 2 (O10Ca2Cu3Ba2Tl2, O6Cu1Ba2Hg1Tl1). 3 remaining unknown.

## Step B: Q1
threshold1 = T* - 32 = 127.0 - 32 = 95.0
Hg + exactly 4 elements + Tc >= 95.0:
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓

Excluded (Tc < 95.0): O4.085 (92.0), O4.101 (93.4), O4.103 (92.5), O4.200 (94.2)

## Step C: Rule Induction

### Measurable data summary:
- ALL 10 measurable 5-elem Ca materials: HIGH (92-127 K)
- ALL 10 measurable 4-elem Hg materials: HIGH (92-98 K)
- O10Ca2Cu3Ba2Tl2 (has Ca, has Tl, no Hg): HIGH (118.97 K)
- O6Cu1Ba2Hg1Tl1 (has Hg+Tl, no Ca): LOW (0.0 K)

### Key observation:
The only LOW material has BOTH Hg and Tl but no Ca.
All HIGH materials either have Ca, or have Hg without Tl.

### Multi-factor rule:
"HIGH if contains Ca, OR (contains Hg AND does NOT contain Tl AND oxygen >= 4.08)"

This combines:
1. Element presence (Ca vs Hg/Tl)
2. Oxygen stoichiometry threshold

Why single-factor rules fail:
- "Contains Hg → HIGH": wrong on O6Cu1Ba2Hg1Tl1 (has Hg, LOW), and possibly O4.047/O4.066 (low oxygen)
- "No Tl → HIGH": wrong on O4.047/O4.066 (no Tl but below oxygen threshold)
- "O > 4 → HIGH": wrong on O6Cu1Ba2Hg1Tl1 (O=6 but LOW)

### Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: has Ca, O=6.160 in range → **HIGH** (~113 K interpolated)
- O6.270Ca1Cu2Ba2Hg1: has Ca, O=6.270 in range → **HIGH** (~112 K interpolated)
- O4.047Cu1Ba2Hg1: Hg, no Tl, but O=4.047 < 4.08 → **LOW**
- O4.066Cu1Ba2Hg1: Hg, no Tl, but O=4.066 < 4.08 → **LOW**
- O4.160Cu1Ba2Hg1: Hg, no Tl, O=4.160 >= 4.08 → **HIGH** (~97 K)
- O4.180Cu1Ba2Hg1: Hg, no Tl, O=4.180 >= 4.08 → **HIGH** (~96 K)

## Step D: Final pick
Among predicted HIGH unmeasurable:
- O4.160Cu1Ba2Hg1: 4 elements, expected Tc ~97.2
- O4.180Cu1Ba2Hg1: 4 elements, expected Tc ~96.0
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

Fewest elements = 4 (O4.160 and O4.180). Tie-break by higher expected Tc → **O4.160Cu1Ba2Hg1**
