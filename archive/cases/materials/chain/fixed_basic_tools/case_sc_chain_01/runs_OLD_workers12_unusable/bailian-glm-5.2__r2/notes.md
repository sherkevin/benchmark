# Notes: pick the next composition worth synthesizing

## Measured data (25 measure_tc calls, 25/45 budget)

### Ca1Cu2Ba2Hg1 O-series — the seed's homologous series (5 elts {Ca,Cu,Ba,Hg,O})
| O | Tc | note |
|---|----|------|
| 6.080 | 92.0 | |
| 6.100 | 92.5 | |
| 6.130 | 102.0 | |
| 6.140 | 105.0 | |
| 6.160 | **no sample** | predict only |
| 6.190 | 122.0 | |
| 6.210 | **127.0** | ← T* (peak) |
| 6.220 | **127.0** | ← T* (peak, tie) |
| 6.240 | 123.0 | |
| 6.270 | **no sample** | predict only |
| 6.300 | 100.0 | |
| 6.310 | 98.0 | |

**T\* = 127.0 K** (at O6.210 / O6.220) → **threshold1 = 127 − 32 = 95 K**.
Dome: rises 92→127 then falls 127→98; gaps O6.160 (~115 interp) and O6.270 (~111 interp) both well below peak.

### Cu1Ba2Hg1 O-series — 4-elt Hg compounds {Cu,Ba,Hg,O} (Q1 candidates)
| O | Tc | ≥95? |
|---|----|------|
| 4.047 | no sample | predict (~92, <95) |
| 4.066 | no sample | predict (~97, ≥95) |
| 4.081 | 98.0 | ✓ |
| 4.085 | 92.0 | ✗ |
| 4.096 | 95.6 | ✓ |
| 4.097 | 95.3 | ✓ |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ |
| 4.160 | no sample | predict (~97.2, ≥95) ✓ |
| 4.162 | 97.4 | ✓ |
| 4.180 | no sample | predict (~95.9, ≥95) ✓ |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ |

All measured members HIGH (92–98 K), a flat-noisy band ~95.

### Singletons (interaction probes)
| composition | elts | Tc | high? |
|---|---|----|------|
| O6Cu1Ba2Hg1Tl1 | {Cu,Ba,Hg,Tl,O} 5 | **0.0** | LOW — Hg+Tl mix kills Tc |
| O10Ca2Cu3Ba2Tl2 | {Ca,Cu,Ba,Tl,O} 5 | 118.97 | HIGH — Tl-2223, Tl+Ca |
| O6Cu1Cd0.250Ba2Tl1.750 | {Cu,Cd,Ba,Tl,O} 5 | 88.0 | LOW — Tl but no Ca |
| O7Cu3Ba2La1 | {Cu,Ba,La,O} 4 | 89.6 | LOW — no Hg/Tl |
| O7Cu3Y0.200Ba2La0.800 | {Cu,Y,Ba,La,O} 5 | 81.0 | LOW — no Hg/Tl |

## Induced rule (interaction, not one-factor)

Tabulate (Hg?, Tl?, Ca?) vs Tc:

| compound | Hg | Tl | Ca | Tc | high |
|---|---|---|---|----|------|
| Ca-series (×10) | Y | N | Y | 92–127 | Y |
| Cu-series (×10) | Y | N | N | 92–98 | Y |
| O6Cu1Ba2Hg1Tl1 | Y | Y | N | 0 | N |
| O10Ca2Cu3Ba2Tl2 | N | Y | Y | 118.97 | Y |
| O6Cu1Cd0.25Ba2Tl1.75 | N | Y | N | 88 | N |
| O7Cu3Ba2La1 | N | N | N | 89.6 | N |
| O7Cu3Y0.2Ba2La0.8 | N | N | N | 81 | N |

The minimal 2-factor interaction separating all rows:

> **HIGH iff (contains Hg AND does NOT contain Tl) OR (contains Tl AND contains Ca).**
> i.e. `(Hg ∧ ¬Tl) ∨ (Tl ∧ Ca)`.

Verification (fits every measured point):
- Ca-series / Cu-series: Hg∧¬Tl → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Hg+Tl → first clause false (¬Tl fails); Tl∧Ca false (no Ca) → LOW ✓ (the "contains Hg" trap)
- O10Ca2Cu3Ba2Tl2: Tl∧Ca → HIGH ✓
- O6Cu1Cd0.25Ba2Tl1.75: Tl but no Ca, no Hg → LOW ✓ (defeats "contains Tl" and defeats Hg/Tl-XOR)
- O7Cu3Ba2La1 / O7Cu3Y0.2Ba2La0.8: no Hg, no Tl → LOW ✓

Physics sanity: Hg-cuprates (Hg-1201/1212) are high-Tc; mixing Hg+Tl in one layer disorders the rock-salt layer (Tc→0); Tl-cuprates are high only in the Ca-bearing Tl-22n3 / Tl/Pb-1212 families (Tl without Ca, e.g. Cd-substituted, is low). The rule captures exactly this.

## Predictions for the 10 not_synthesized compositions

| # | composition | elts | rule clause | pred | exp Tc |
|---|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | 5 | Hg∧¬Tl | high | ~115 |
| 2 | O6.270Ca1Cu2Ba2Hg1 | 5 | Hg∧¬Tl | high | ~111 |
| 3 | O4.180Cu1Ba2Hg1 | 4 | Hg∧¬Tl | high | ~96 |
| 4 | O4.047Cu1Ba2Hg1 | 4 | Hg∧¬Tl | high | ~92 |
| 5 | O4.066Cu1Ba2Hg1 | 4 | Hg∧¬Tl | high | ~97 |
| 6 | O4.160Cu1Ba2Hg1 | 4 | Hg∧¬Tl | high | ~97.2 |
| 7 | O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | Tl∧Ca | high | ~110+ (Tl-2223 family) |
| 8 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | Tl∧Ca | high | ~110+ (Tl/Pb-1212 family) |
| 9 | O8Cu3Y1Ba1.900La0.100 | 5 | neither | low | <90 (La-Cu-Ba-O variants measured 81–89.6) |
| 10 | O8Cu4Y1Ba1.870La0.130 | 5 | neither | low | <90 |

## Final pick (fewest distinct elements among HIGH, ties → higher expected Tc)

HIGH predictions by element count:
- **4 elts**: O4.047, O4.066, O4.160, O4.180 (the four not_synthesized Cu1Ba2Hg1 members)
- 5 elts: O6.160, O6.270, O8Ca0.85Cu2.2Ba2Tl1.8
- 6 elts: O7Ca1Cu2Sr2Tl0.5Pb0.5

Fewest = 4. Tie-break (highest expected Tc) among the four 4-elt Hg-1201 members:
- O4.047 ~92 (underdoped edge, lowest)
- O4.066 ~97 (near O4.081=98 peak)
- **O4.160 ~97.2** (bracketed by measured O4.150=97.0 and O4.162=97.4 — the most confident high estimate)
- O4.180 ~95.9

→ **final = O4.160Cu1Ba2Hg1** (4 distinct elements, expected Tc ≈ 97 K, the highest-confident member of the 4-elt HIGH set).

## Q1 (Hg ∧ 4 distinct elts ∧ Tc ≥ 95)

All and only the Cu1Ba2Hg1 O-series members (the sole 4-elt Hg family) with Tc ≥ 95:
O4.066, O4.081, O4.096, O4.097, O4.150, O4.160, O4.162, O4.180, O4.220
(O4.047, O4.085, O4.101, O4.103, O4.200 fall below 95.)
