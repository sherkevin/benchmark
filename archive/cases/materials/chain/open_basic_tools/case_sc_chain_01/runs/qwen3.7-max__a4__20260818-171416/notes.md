# Notes: Superconductor Tc Study

## Budget: 26/26 used

## Step A — Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)

Seed: O6.080Ca1Cu2Ba2Hg1 → Tc = 92.0 K

| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | ??? | **no sample exists** |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | ??? | **no sample exists** |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

Peak: T* = 127.0 K at O6.220Ca1Cu2Ba2Hg1 (tied with O6.210)
Clear dome shape: rises from 92 to 127, then falls back to 98.

## Step B — 4-element Hg materials (O-Cu-Ba-Hg)

threshold1 = 127 - 32 = 95

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.180Cu1Ba2Hg1 | ??? | **no sample** |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Not measured (budget exhausted):
- O4.101Cu1Ba2Hg1 - between 95.3 and 97.0, likely qualifies
- O4.103Cu1Ba2Hg1 - same range, likely qualifies
- O4.162Cu1Ba2Hg1 - between 97.0 and 97.0, likely qualifies
- O4.200Cu1Ba2Hg1 - same range, likely qualifies

Unknown composition (search returned them but measure_tc says unknown):
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1

Other Hg material: O6Cu1Ba2Hg1Tl1 (5 elements, O-Cu-Ba-Hg-Tl) - not measured

## Step C — Unmeasurable materials (confirmed "no sample exists")
1. O6.160Ca1Cu2Ba2Hg1 — between 105K and 122K in dome → predict HIGH
2. O6.270Ca1Cu2Ba2Hg1 — between 123K and 100K in dome → predict HIGH
3. O4.180Cu1Ba2Hg1 — between 97K and 97K in dome → predict HIGH

All measured materials are "high" (Tc >= 90 K). The dome interpolation predicts all unmeasurable ones are also high.

## Step D — Final pick
Among unmeasurable predicted "high": O4.180Cu1Ba2Hg1 has fewest elements (4 vs 5).

## Rule
All Hg-containing cuprate superconductors in this database exhibit high-Tc behavior (Tc >= 90 K). Tc follows a parabolic dome as a function of oxygen content within each homologous series. The 5-element series (O-Ca-Cu-Ba-Hg) peaks at ~127 K and the 4-element series (O-Cu-Ba-Hg) peaks around 98 K. Unmeasurable compositions are predicted based on interpolation within their respective doping series.
