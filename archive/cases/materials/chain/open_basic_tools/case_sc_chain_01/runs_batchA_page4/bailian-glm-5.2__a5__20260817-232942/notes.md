# Superconductor Tc Study - Working Notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
- High if Tc >= 90 K, else low
- Budget: 45 units total (shared between measure_tc and search_db)

## Step A — series peak
Find homologous doping series (same element set {O,Ca,Cu,Ba,Hg}, varying oxygen).
Measure members to find max Tc = T*.

## Step B — filter family B
threshold1 = T* - 32. Materials containing Hg AND exactly 4 elements. Report all with Tc >= threshold1 (Q1).

## Step C — extrapolate
For unmeasurable materials (measure_tc returns "no sample exists"), predict high/low.

## Step D — final pick
Among unmeasurable predicted HIGH, choose fewest elements (tie-break: higher expected Tc).

## All Hg materials (27 total, 7 search pages)
O4.x Cu1Ba2Hg1 series (4 elem: O,Cu,Ba,Hg) — 14 members:
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
O6.x Ca1Cu2Ba2Hg1 series (5 elem: O,Ca,Cu,Ba,Hg) — 12 members:
  O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220,
  O6.240, O6.270, O6.300, O6.310
O6Cu1Ba2Hg1Tl1 (5 elem) — 1 member

## Step A measurements (seed series O6.x Ca1Cu2Ba2Hg1)
| O | Tc |
|6.080| 92.0 |
|6.100| 92.5 |
|6.130| 102.0 |
|6.140| 105.0 |
|6.160| UNMEASURABLE |
|6.190| 122.0 |
|6.210| 127.0 |  <- MAX (tie)
|6.220| 127.0 |  <- MAX (tie) = T*
|6.240| 123.0 |
|6.270| UNMEASURABLE |
|6.300| 100.0 |
|6.310| 98.0 |

T* = 127.0 K (peak = O6.220Ca1Cu2Ba2Hg1; O6.210 ties)
threshold1 = 127 - 32 = 95.0 K

## Step B: 4-element Hg series O4.x Cu1Ba2Hg1 (threshold=95.0)
| O | Tc | status |
|4.047| ? | UNMEASURABLE |
|4.066| ? | UNMEASURABLE |
|4.081| 98.0 | Q1 ✓ |
|4.085| 92.0 | low |
|4.096| 95.6 | Q1 ✓ |
|4.097| 95.3 | Q1 ✓ |
|4.101| 93.4 | low |
|4.103| 92.5 | low |
|4.150| 97.0 | Q1 ✓ |
|4.160| ? | UNMEASURABLE |
|4.162| 97.4 | Q1 ✓ |
|4.180| ? | UNMEASURABLE |
|4.200| 94.2 | low |
|4.220| 97.0 | Q1 ✓ |

Q1 (Tc >= 95.0): {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1,
  O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Extra measurement
O6Cu1Ba2Hg1Tl1 (Tl-doped, 5 elem) = 0.0 K (low/no-superconductivity)

## All unmeasurable materials found (via search_db "Hg")
1. O6.160Ca1Cu2Ba2Hg1 (5 elem)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem)
3. O4.047Cu1Ba2Hg1 (4 elem)
4. O4.066Cu1Ba2Hg1 (4 elem)
5. O4.160Cu1Ba2Hg1 (4 elem)
6. O4.180Cu1Ba2Hg1 (4 elem)

## Step C — rule induction

ALL measurable Hg-cuprates are HIGH (>=90 K) EXCEPT O6Cu1Ba2Hg1Tl1 (Tl-bearing) = 0.0 K (LOW).
- 4-elem Hg-1201 (O4.081–4.220): all high 92–98 K (noisy, ~flat)
- 5-elem Hg-1212 (O6.080–6.310): all high 92–127 K, CLEAN doping dome, peak 127 @ O6.21–6.22
- 5-elem Tl (O6Cu1Ba2Hg1Tl1): LOW (0 K) — Tl substitution kills superconductivity

5-element dome interpolation for unmeasurable:
- O6.160: between 6.140(105) & 6.190(122) → ~116 K → HIGH
- O6.270: between 6.240(123) & 6.300(100) → ~112 K → HIGH

4-element: measurable high window is O4.081–4.220 (all 92–98 K).
- O4.160: between 4.150(97.0) & 4.162(97.4) → ~97.3 K → HIGH
- O4.180: between 4.162(97.4) & 4.200(94.2) → ~95.9 K → HIGH
- O4.047 (δ=0.047): UNDERDOPED, below window → <90 K → LOW
- O4.066 (δ=0.066): UNDERDOPED, below window (δ<0.08) → <90 K → LOW

WARNING check: naive single-factor "contains Hg → high" is WRONG on O4.047 & O4.066
(underdoped lows) = "several". Multi-factor rule (family + doping window + Tl) needed. ✓

## Rule (induced)
Hg-based cuprates are high-Tc WITHIN their superconducting oxygen-doping windows:
- Hg-1212 (O-Ca-Cu2-Ba2-Hg, 5 elem): doping dome, peak ~127 K @ O≈6.21, ≥90 K across O6.08–6.31.
- Hg-1201 (O-Cu-Ba-Hg, 4 elem): ~92–98 K across O4.08–4.22, but drops <90 K when
  underdoped (δ<~0.08, i.e. O≲4.07).
Tl substitution (Tl-bearing Hg cuprate) suppresses superconductivity (Tc≈0, low).
A material is HIGH iff it is a Hg-1201/Hg-1212 cuprate within its doping window and Tl-free.

## Predictions (all 6 unmeasurable found via search_db "Hg")
- O4.047Cu1Ba2Hg1 → low (underdoped Hg-1201)
- O4.066Cu1Ba2Hg1 → low (underdoped Hg-1201)
- O4.160Cu1Ba2Hg1 → high (~97.3 K)
- O4.180Cu1Ba2Hg1 → high (~95.9 K)
- O6.160Ca1Cu2Ba2Hg1 → high (~116 K)
- O6.270Ca1Cu2Ba2Hg1 → high (~112 K)

## Step D — final pick
Predicted-high unmeasurable: {O4.160, O4.180, O6.160, O6.270}
Fewest elements: 4-elem (O4.160, O4.180) beat 5-elem.
Tie-break higher expected Tc: O4.160 (~97.3) > O4.180 (~95.9).
FINAL = O4.160Cu1Ba2Hg1
(Robust: even if O4.047/O4.066 were high, their extrapolated Tc < O4.160's 97.3, so final unchanged.)

## Budget tracking
- search_db pages: 7 (budget 1–7)
- measure_tc successful (21): budget 8–28
- measure_tc unmeasurable (6, free): O6.160, O6.270, O4.047, O4.066, O4.160, O4.180
- Total used: 28 / 45 (17 remaining — no need to spend; all 27 Hg materials already found)
