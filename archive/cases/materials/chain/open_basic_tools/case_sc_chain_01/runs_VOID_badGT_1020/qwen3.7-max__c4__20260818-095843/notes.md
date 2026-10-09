# Notes - Superconductor Tc Study

## Budget Usage
- Total budget: 34 units
- Searches: 14 (4 series pages + 9 Hg pages + 1 seed measurement counted separately)
- Measurements: 20 (including unmeasurable attempts that were free)
- Budget fully exhausted at 34/34

## Step A — Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
12 members found via search_db("Ca1Cu2Ba2Hg"):

| Composition | Oxygen | Tc (K) | Sample? | High? |
|---|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | yes | high |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | yes | high |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | yes | high |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | yes | high |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | — | NO SAMPLE | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | yes | high |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | 127.0 | yes | high |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | 127.0 | yes | high |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | yes | high |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | — | NO SAMPLE | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | yes | high |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | yes | high |

**Peak: T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)
Tc dome shape: rises from 92 → 127 → drops to 98 as oxygen increases.

## Step B — Hg + exactly 4 elements, Tc >= threshold1 = 127 - 32 = 95.0

14 four-element materials (O, Cu, Ba, Hg) found in Hg search:

| Composition | Tc (K) | Sample? | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO SAMPLE | unmeasurable |
| O4.066Cu1Ba2Hg1 | — | NO SAMPLE | unmeasurable |
| O4.081Cu1Ba2Hg1 | 98.0 | yes | YES ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | yes | no |
| O4.096Cu1Ba2Hg1 | 95.6 | yes | YES ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | yes | YES ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | yes | no |
| O4.103Cu1Ba2Hg1 | 92.5 | yes | no |
| O4.150Cu1Ba2Hg1 | 97.0 | yes | YES ✓ |
| O4.160Cu1Ba2Hg1 | — | NO SAMPLE | unmeasurable |
| O4.162Cu1Ba2Hg1 | 97.4 | yes | YES ✓ |
| O4.180Cu1Ba2Hg1 | — | NO SAMPLE | unmeasurable |
| O4.200Cu1Ba2Hg1 | 94.2 | yes | no |
| O4.220Cu1Ba2Hg1 | 97.0 | yes | YES ✓ |

**Q1 = 6 materials** with Tc >= 95.0 K

Also found: O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl) — Tc = 0.0 K (LOW, not superconducting)

## Step C — Unmeasurable materials and prediction rule

6 unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) — series member, between O6.140(105) and O6.190(122)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) — series member, between O6.240(123) and O6.300(100)
3. O4.047Cu1Ba2Hg1 (4 elements) — below measured range
4. O4.066Cu1Ba2Hg1 (4 elements) — below measured range
5. O4.160Cu1Ba2Hg1 (4 elements) — between O4.150(97.0) and O4.162(97.4)
6. O4.180Cu1Ba2Hg1 (4 elements) — between O4.162(97.4) and O4.200(94.2)

### Rule analysis:
- ALL measurable 4-element (O,Cu,Ba,Hg) materials: HIGH (10/10, Tc 92-98 K)
- ALL measurable 5-element (O,Ca,Cu,Ba,Hg) series: HIGH (10/10, Tc 92-127 K)
- Tl-containing material: LOW (Tc = 0 K)

**Multi-factor rule**: Hg-containing cuprate materials with Ba-Cu-O framework are high-Tc (Tc ≥ 90 K) when they have standard cuprate layer structure: 4 elements (O-Cu-Ba-Hg) or 5 elements with Ca (O-Ca-Cu-Ba-Hg). Presence of Tl disrupts superconductivity. Both element composition AND structural arrangement matter.

**Predictions**: All 6 unmeasurable → HIGH (same element sets as measurable high-Tc materials)

## Step D — Final pick

Among unmeasurable predicted high:
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1

Fewest elements = 4. Tie-break by higher expected Tc:
- O4.160: between O4.150(97.0) and O4.162(97.4) → expected ~97.2 K (highest)
- O4.180: between O4.162(97.4) and O4.200(94.2) → expected ~95.8 K
- O4.066: near O4.081(98.0) → expected ~96-97 K
- O4.047: furthest below range → expected ~94-96 K

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ≈ 97.2 K)
