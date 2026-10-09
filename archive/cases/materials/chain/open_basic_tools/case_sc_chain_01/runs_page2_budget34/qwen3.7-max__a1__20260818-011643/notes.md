# Notes - Superconductor Tc Study

## Seed material: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements)
- Homologous series: same element set, varying oxygen

## Budget tracking
- Total: 34 units
- Used: 34 (all spent)
  - Searches: 14 units (6 for series, 8 for Hg search)
  - Measurements: 20 units (12 series + 8 other)

## Step A: Series Results (O6.xxxCa1Cu2Ba2Hg1)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable, HIGH |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measurable, HIGH |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measurable, HIGH |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measurable, HIGH |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable, HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measurable, HIGH ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measurable, HIGH ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable, HIGH |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measurable, HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measurable, HIGH |

**T* = 127.0 K** (O6.210Ca1Cu2Ba2Hg1 chosen as peak)

## Step B: 4-element Hg materials (O4.xxxCu1Ba2Hg1)
threshold1 = 127 - 32 = 95

| Composition | Tc (K) | Status | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | — | UNMEASURABLE | — |
| O4.066Cu1Ba2Hg1 | — | UNMEASURABLE | — |
| O4.081Cu1Ba2Hg1 | 98.0 | measurable, HIGH | YES ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | measurable, HIGH | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | measurable, HIGH | YES ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | measurable, HIGH | YES ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | measurable, HIGH | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | measurable, HIGH | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | measurable, HIGH | YES ✓ |
| O4.160Cu1Ba2Hg1 | — | UNMEASURABLE | — |
| O4.162Cu1Ba2Hg1 | — | likely UNMEASURABLE | — |
| O4.180Cu1Ba2Hg1 | — | likely UNMEASURABLE | — |
| O4.200Cu1Ba2Hg1 | — | likely UNMEASURABLE | — |
| O4.220Cu1Ba2Hg1 | — | likely UNMEASURABLE | — |

**Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1}**

## Other materials found
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K, LOW (5 elements: O, Cu, Ba, Hg, Tl) — only LOW material!

## Step C: Rule Induction

### Measured data summary:
- ALL materials with Ca are HIGH (Tc 92-127)
- ALL 4-element materials (O, Cu, Ba, Hg) are HIGH (Tc 92-98)
- O6Cu1Ba2Hg1Tl1 (5 elements, no Ca, has Tl) is LOW (Tc = 0)

### Rule: "High if contains Ca OR has exactly 4 elements; otherwise low."
- Multi-factor rule (element composition + element count)
- Single-factor rules fail: "has Ca → high" wrong on 4-element materials; "5 elements → high" wrong on Tl material

### Unmeasurable materials and predictions:
1. O6.160Ca1Cu2Ba2Hg1 → has Ca → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 → has Ca → **HIGH**
3. O4.047Cu1Ba2Hg1 → 4 elements → **HIGH**
4. O4.066Cu1Ba2Hg1 → 4 elements → **HIGH**
5. O4.160Cu1Ba2Hg1 → 4 elements → **HIGH**
6. O4.162Cu1Ba2Hg1 → 4 elements → **HIGH**
7. O4.180Cu1Ba2Hg1 → 4 elements → **HIGH**
8. O4.200Cu1Ba2Hg1 → 4 elements → **HIGH**
9. O4.220Cu1Ba2Hg1 → 4 elements → **HIGH**

## Step D: Final Pick
Among predicted-high unmeasurable materials:
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Fewest elements = 4. Tie-break: highest expected Tc.
- O4.160Cu1Ba2Hg1 is closest to measured range (near O4.150 at 97.0), expected highest Tc.

**Final pick: O4.160Cu1Ba2Hg1**
