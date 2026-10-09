# Superconductor Tc study — notes

## Budget: 34 total; used 25 (no-sample calls don't consume budget).

## Step A — series O6.xxxCa1Cu2Ba2Hg1 (Hg-1212, 5 el, 2 Cu, Ca)
| composition | O | Tc | |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | H |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | H |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | H |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | H |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | NO SAMPLE (in unmeasurable) | |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | H |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | **127.0** | H ★ peak |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | **127.0** | H ★ peak (tie) |
| O6.240Ca1Cu2Ba2Ba2Hg1 | 6.240 | 123.0 | H |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | NO SAMPLE (in unmeasurable) | |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | H |
| O6.310Ca1Cu2Ba2Ba2Hg1 | 6.310 | 98.0 | H |

**T* = 127.0 K** (peak composition O6.210Ca1Cu2Ba2Hg1; O6.220 ties).
Bell curve, peak ~O6.21–6.22; all measured members HIGH.

## Step B — O4.xxxCu1Ba2Hg1 (Hg-1201, 4 el, 1 Cu, no Ca) — contains Hg & exactly 4 elements
| composition | O | Tc | ≥95? |
|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 4.081 | 98.0 | ✓ Q1 |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | ✓ Q1 |
| O4.150Cu1Ba2Hg1 | 4.150 | 97.0 | ✓ Q1 |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | ✓ Q1 |
| O4.096Cu1Ba2Hg1 | 4.096 | 95.6 | ✓ Q1 |
| O4.097Cu1Ba2Hg1 | 4.097 | 95.3 | ✓ Q1 |
| O4.200Cu1Ba2Hg1 | 4.200 | 94.2 | ✗ |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 4.103 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 4.085 | 92.0 | ✗ |
| O4.180 / O4.047 | — | NO SAMPLE (in unmeasurable) | — |

**threshold1 = T* − 32 = 127 − 32 = 95.0**
**Q1** (measured Tc ≥ 95.0, 4-el & has Hg) = {O4.081, O4.162, O4.150, O4.220, O4.096, O4.097}

## Step C — cross-family measurements (to induce rule)
| composition | family | Cu | O | O/Cu | Tc | label |
|---|---|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | Hg+Tl 1Cu O6 | 1 | 6 | 6.00 | 0.0 | LOW |
| O10Ca2Cu3Ba2Tl2 | Tl-2223 | 3 | 10 | 3.33 | 118.97 | HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | Tl+Cd 1Cu O6 | 1 | 6 | 6.00 | 88.0 | LOW |
| O7Cu3Ba2La1 | YBCO-like Cu3 O7 | 3 | 7 | 2.33 | 89.6 | LOW |
| O7Cu3Y0.200Ba2La0.800 | YBCO-like Cu3 O7 | 3 | 7 | 2.33 | 81.0 | LOW |

### Rule induction
Single-factor rules fail: "contains Hg" wrongly marks O6Cu1Ba2Hg1Tl1 (Hg, Tc=0) high; "has 5 elements" wrongly marks O6Cu1Ba2Hg1Tl1 (5el, 0K) & O7Cu3Y0.2Ba2La0.8 (5el, 81K) high.

**Empirical interaction rule:** HIGH Tc requires oxygen content appropriate to the number of CuO2 layers (optimal hole doping). Measured HIGH materials all have O/Cu ≈ 3.0–4.5:
- Hg-1201 (1 Cu): O/Cu 4.05–4.22 → 92–98 K (HIGH)
- Hg-1212 (2 Cu): O/Cu 3.04–3.16 → 92–127 K (HIGH)
- Tl-2223 (3 Cu): O/Cu 3.33 → 119 K (HIGH)
LOW materials fall outside this band:
- Overdoped: O6 single-Cu Hg/Tl & Hg+Tl (O/Cu = 6.00) → 0–88 K (LOW)
- Underdoped: O7 Cu3 La/Y (O/Cu = 2.33) → 81–89.6 K (LOW); for 3 Cu the optimal is O10 (Tl-2223), so O7/O8 Cu3 is underdoped.

So the governing rule is the **interaction of oxygen content with Cu-layer count (O per CuO2 layer)** — i.e. whether the hole doping is in the optimal band, not the mere presence of an element or the element count.

### Predictions for 10 unmeasurable
| # | composition | Cu | O | O/Cu | predicted | expected Tc (anchor) |
|---|---|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | 2 | 6.160 | 3.080 | HIGH | ~112 (btwn 6.140=105 & 6.190=122) |
| 2 | O6.270Ca1Cu2Ba2Hg1 | 2 | 6.270 | 3.135 | HIGH | ~111 (btwn 6.240=123 & 6.300=100) |
| 3 | O4.180Cu1Ba2Hg1 | 1 | 4.180 | 4.180 | HIGH | ~96 (btwn 4.162=97.4 & 4.200=94.2) |
| 4 | O4.047Cu1Ba2Hg1 | 1 | 4.047 | 4.047 | HIGH | ~94 (just below peak 4.081=98) |
| 5 | O4.066Cu1Ba2Hg1 | 1 | 4.066 | 4.066 | HIGH | ~96 (near peak 4.081=98) |
| 6 | O4.160Cu1Ba2Hg1 | 1 | 4.160 | 4.160 | HIGH | ~97.3 (btwn 4.150=97.0 & 4.162=97.4) |
| 7 | O8Ca0.850Cu2.200Ba2Tl1.800 | 2.2 | 8 | 3.636 | HIGH | Tl-2212 @ stoich O8 (~90–110) |
| 8 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | 2 | 7 | 3.500 | HIGH | (Tl,Pb)-1212 2Cu O7 (~90) |
| 9 | O8Cu3Y1Ba1.900La0.100 | 3 | 8 | 2.667 | LOW | Cu3 needs O~10; O8 underdoped |
| 10 | O8Cu4Y1Ba1.870La0.130 | 4 | 8 | 2.000 | LOW | Cu4 needs O~13; O8 far underdoped |

8 HIGH (items 1–8), 2 LOW (items 9, 10).

## Step D — final pick
Predicted-HIGH with FEWEST elements:
- items 1,2,7 = 5 elements; item 8 = 6 elements;
- items 3,4,5,6 = 4 elements (Hg-1201) → tie.
Break tie by higher expected Tc:
- item 6 O4.160 ~97.3 (highest; sits at measured 4.150=97.0/4.162=97.4)
- item 5 O4.066 ~96; item 3 O4.180 ~96; item 4 O4.047 ~94 (lowest, furthest from peak)

**Final = O4.160Cu1Ba2Hg1**
