# Notes: Superconductor Tc Study

## Task Overview
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
- Budget: 45 units total (shared between measure_tc and search_db)
- High = Tc >= 90 K, Low = Tc < 90 K

## DB Discovery
Searches run: Hg (pages 1-14, all 27 matches), Tl (pages 1-3, all 5 matches), Ca (pages 1-2, subset).

All Hg materials (27):
- 14 four-element (O,Cu,Ba,Hg) Cu1Ba2Hg1: O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
- 12 five-element series (O,Ca,Cu,Ba,Hg) Ca1Cu2Ba2Hg1: O6.080..O6.310
- 1 five-element: O6Cu1Ba2Hg1Tl1

All Tl materials (5):
- O10Ca2Cu3Ba2Tl2 (Tl-2223 family)
- O6Cu1Ba2Hg1Tl1 (Hg+Tl)
- O6Cu1Cd0.250Ba2Tl1.750 (Cd-doped Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (Sr-based Tl-1212)
- O8Ca0.850Cu2.200Ba2Tl1.800 (Tl-2212-ish)

## Step A — Series Peak (homologous doping series, same elements, varying O)
Series = O,Ca,Cu,Ba,Hg with O6.xxx. Measured Tc:
| composition | Tc | sample? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes (seed) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | **NO SAMPLE** | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | **NO SAMPLE** | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**T* = 127.0** (peak, O6.210 and O6.220 tied at 127.0). Report peak = O6.210Ca1Cu2Ba2Hg1.

## Step B — Q1 (Hg + exactly 4 elements, Tc >= threshold1)
threshold1 = 127.0 - 32 = **95.0**
| composition | Tc | sample? | >=95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO | predict |
| O4.066Cu1Ba2Hg1 | — | NO | predict |
| O4.081Cu1Ba2Hg1 | 98.0 | yes | ✓ Q1 |
| O4.085Cu1Ba2Hg1 | 92.0 | yes | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | yes | ✓ Q1 |
| O4.097Cu1Ba2Hg1 | 95.3 | yes | ✓ Q1 |
| O4.101Cu1Ba2Hg1 | 93.4 | yes | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | yes | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | yes | ✓ Q1 |
| O4.160Cu1Ba2Hg1 | — | NO | predict |
| O4.162Cu1Ba2Hg1 | 97.4 | yes | ✓ Q1 |
| O4.180Cu1Ba2Hg1 | — | NO | predict |
| O4.200Cu1Ba2Hg1 | 94.2 | yes | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | yes | ✓ Q1 |

**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1** (all measured, all ≥95.0)
(Q1 is "report ALL whose Tc >= threshold1" — only measurable ones have measured Tc; unmeasurable go to Step C predictions, not Q1.)

## Step C — Unmeasurable materials (predict high/low) — ALL discovered
8 unmeasurable materials found via search_db:

### Hg family (interpolate from measured neighbors in same family)
1. O6.160Ca1Cu2Ba2Hg1 (5 elem): between O6.140(105.0) & O6.190(122.0) → ~113 → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 (5 elem): between O6.240(123.0) & O6.300(100.0) → ~112 → **HIGH**
3. O4.047Cu1Ba2Hg1 (4 elem): lowest O in 4-el family; family all 92-98 → ~93-95 → **HIGH**
4. O4.066Cu1Ba2Hg1 (4 elem): just below O4.081(98.0) → ~96-98 → **HIGH**
5. O4.160Cu1Ba2Hg1 (4 elem): between O4.150(97.0) & O4.162(97.4) → ~97.2 → **HIGH**
6. O4.180Cu1Ba2Hg1 (4 elem): between O4.162(97.4) & O4.200(94.2) → ~96 → **HIGH**

### Tl family (structure-based; reference measured: Tl-2223 O10Ca2Cu3Ba2Tl2=118.97 high; O6Cu1Cd0.25Ba2Tl1.75=88.0 low; O6Cu1Ba2Hg1Tl1=0.0 low)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem): (Tl,Pb)Sr2CaCu2O7 = Tl-1212 Sr-based, known ~80K, Pb/Tl half-site & Sr (not Ba) lower Tc → **LOW**
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem): Tl2Ba2CaCu2O8+δ = Tl-2212 family, ~105-108K → **HIGH**

## Step D — Final pick among predicted-HIGH unmeasurable
Predicted HIGH unmeasurable: #1,#2 (5 elem), #3,#4,#5,#6 (4 elem), #8 (5 elem).
Fewest elements = 4 (candidates: O4.047, O4.066, O4.160, O4.180).
Tie-break higher expected Tc:
- O4.160 → ~97.2 (highest, sandwiched between 97.0 & 97.4)
- O4.180 → ~96
- O4.066 → ~96-98
- O4.047 → ~93-95 (lowest)
**Final pick = O4.160Cu1Ba2Hg1** (4 elements, ~97.2K expected)

## Rule (induced)
Multi-factor: predict Tc by interpolating within the material's homologous family (same element set, varying oxygen/stoichiometry) from measured neighbors; where no measured family neighbor exists, use the cuprate structure–Tc relationship: layer count (Cu2/Cu3 with Ba+Ca charge reservoir → high Tc ~100-128K) vs single-Cu-layer or Sr-based / heavily-substituted / half-site (Tl,Pb) layers → lower Tc (<90K). HIGH iff estimated Tc >= 90K.
Why single-factor fails: oxygen-value alone misses the peak (low O & high O both can be high or low depending on family); Cu-count alone wrongly marks Sr-based Tl-1212 (Cu2) as high when it's ~80K; "contains Tl" wrongly marks Cd/Hg co-doped Tl as high when they're 0-88K.

## Budget
Used: 43. Remaining: 2 (held in reserve — all data collected).
