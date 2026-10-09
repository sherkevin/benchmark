# Notes: Superconductor Tc Study

## Budget Usage
- 9 search_db calls (pages 1-9 for "Hg"): 9 units
- 9 measure_tc calls (measurable samples): 9 units  
- 3 unmeasurable attempts (free): 0 units
- Total: 18/18 budget used

## All Hg-Containing Materials Found (27 total)

### Series 1: O-Cu-Ba-Hg (4 elements) — 14 materials
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1,
O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1,
O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

### Series 2: O-Ca-Cu-Ba-Hg (5 elements, seed's homologous series) — 12 materials
O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1,
O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1,
O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

### Other: 1 material
O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl)

## Step A: Series Peak

Measured Tc values for the seed's homologous series (O-Ca-Cu-Ba-Hg):
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable |
| O6.100Ca1Cu2Ba2Hg1 | ? | untested |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measurable |
| O6.140Ca1Cu2Ba2Hg1 | ? | untested |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable |
| O6.210Ca1Cu2Ba2Hg1 | ? | untested |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** | measurable — **PEAK** |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measurable |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measurable |

**Peak: O6.220Ca1Cu2Ba2Hg1, T* = 127.0 K**

The dome shape is clear: rises from 92 K at O=6.080 to peak 127 K at O=6.220, then falls to 98 K at O=6.310.

## Step B: Family B (Hg + exactly 4 elements)

threshold1 = T* - 32 = 127 - 32 = 95 K

4-element Hg materials measured:
- O4.096Cu1Ba2Hg1: 95.6 K ≥ 95 ✓
- O4.150Cu1Ba2Hg1: 97.0 K ≥ 95 ✓

Other 4-element materials (untested):
Based on dome model (peak ~97 K near O=4.12), materials with O between ~4.09 and ~4.17 likely qualify.

Q1 (confirmed): O4.096Cu1Ba2Hg1, O4.150Cu1Ba2Hg1

## Step C: Unmeasurable Materials & Rule

Confirmed unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) — dome interpolation: ~112 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) — dome interpolation: ~112 K → HIGH
3. O4.180Cu1Ba2Hg1 (4 elements) — dome extrapolation: ~93-95 K → HIGH

**Rule**: Multi-factor rule combining element count and oxygen stoichiometry:
- Materials with 5+ elements in the Hg-Ba-Cu-O-Ca system are HIGH (additional CuO2 planes via Ca maintain robust superconductivity across the entire doping range)
- For 4-element Hg-Ba-Cu-O materials: HIGH if oxygen excess per Cu atom is in range [0.08, 0.20], LOW otherwise

All 3 unmeasurable materials predicted HIGH.

## Step D: Final Pick

Among predicted-high unmeasurable materials:
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among high predictions)
