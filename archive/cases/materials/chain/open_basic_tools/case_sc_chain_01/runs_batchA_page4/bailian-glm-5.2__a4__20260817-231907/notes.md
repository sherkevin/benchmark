# Superconductor Tc Study - Working Notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 45 units total (shared between measure_tc and search_db)
- High = Tc >= 90 K, Low = Tc < 90 K
- search_db: 4 results per page, tells if more pages exist

## Search results: ALL Hg materials (27 total, pages 1-7, cost 7)
Series `Ca1Cu2Ba2Hg1` (5 elements: O,Ca,Cu,Ba,Hg), varying O:
  O6.080, O6.100, O6.130, O6.140, O6.160*, O6.190, O6.210, O6.220, O6.240, O6.270*, O6.300, O6.310
  (* = no sample)

O4 family `Cu1Ba2Hg1` (4 elements: O,Cu,Ba,Hg), varying O:
  O4.047*, O4.066*, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160*, O4.162, O4.180*, O4.200, O4.220
  (* = no sample)

Other:
  O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl) — measured 0.0

## Step A — Series peak (Tc by O content), cost 10
| O | Tc | sample |
|---|----|-----|
| 6.080 | 92.0 | yes |
| 6.100 | 92.5 | yes |
| 6.130 | 102.0 | yes |
| 6.140 | 105.0 | yes |
| 6.160 | — | NO |
| 6.190 | 122.0 | yes |
| 6.210 | 127.0 | yes |
| 6.220 | 127.0 | yes |
| 6.240 | 123.0 | yes |
| 6.270 | — | NO |
| 6.300 | 100.0 | yes |
| 6.310 | 98.0 | yes |

Peak T* = 127.0 at O6.210 and O6.220 (tie). Use O6.220 as peak (later O).
Actually both 127.0 — pick O6.220 (max O between the two). Either acceptable. Choose O6.220.

## Step B — threshold1 = T* - 32 = 127.0 - 32.0 = 95.0
Materials with Hg AND exactly 4 elements:
- All O4xxxCu1Ba2Hg1 = 4 elements ✓
- Ca series = 5 elements ✗
- O6Cu1Ba2Hg1Tl1 = 5 elements ✗

Measured O4 family Tc:
| O | Tc | >=95? | sample |
|---|----|------|-----|
| 4.047 | — | ? | NO |
| 4.066 | — | ? | NO |
| 4.081 | 98.0 | yes | yes |
| 4.085 | 92.0 | no | yes |
| 4.096 | 95.6 | yes | yes |
| 4.097 | 95.3 | yes | yes |
| 4.101 | 93.4 | no | yes |
| 4.103 | 92.5 | no | yes |
| 4.150 | 97.0 | yes | yes |
| 4.160 | — | ? | NO |
| 4.162 | 97.4 | yes | yes |
| 4.180 | — | ? | NO |
| 4.200 | 94.2 | no | yes |
| 4.220 | 97.0 | yes | yes |

Q1 (measured, Tc>=95.0): O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0)
Unmeasurable O4 predictions pending Step C: O4.047, O4.066, O4.160, O4.180

## Step C — Unmeasurable materials to predict
From Hg search, unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, series)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, series)
3. O4.047Cu1Ba2Hg1 (4 elem)
4. O4.066Cu1Ba2Hg1 (4 elem)
5. O4.160Cu1Ba2Hg1 (4 elem)
6. O4.180Cu1Ba2Hg1 (4 elem)

Budget used: 28/45. Remaining: 17.

## Rule induction — analysis
Warning: single-factor rules are wrong on several. Need multi-factor.

O4 family (Cu1Ba2Hg1) Tc vs O:
  4.081=98.0, 4.085=92.0, 4.096=95.6, 4.097=95.3, 4.101=93.4, 4.103=92.5, 4.150=97.0, 4.162=97.4, 4.200=94.2, 4.220=97.0
  All >= 92 (high). Range 92-98. No clear monotonic O trend (4.081=98, 4.085=92, 4.150=97, 4.220=97).
  Predictions for missing O4: O4.047, O4.066 (below 4.081), O4.160, O4.180 (between 4.150-4.200).
  All measured O4 are high (>=92). Likely all O4 family are high. → predict high for all 4.

Ca series (Ca1Cu2Ba2Hg1) Tc vs O:
  6.080=92.0, 6.100=92.5, 6.130=102.0, 6.140=105.0, 6.190=122.0, 6.210=127.0, 6.220=127.0, 6.240=123.0, 6.300=100.0, 6.310=98.0
  Bell curve peaked at 6.210-6.220. Missing: 6.160 (between 6.140=105 and 6.190=122 → rising, high), 6.270 (between 6.240=123 and 6.300=100 → declining but still ~110+, high).
  Both missing fall in high region → predict high.

RULE: In Hg-cuprate families, Tc stays in the superconducting (high, >=90K) regime
across the oxygen-doping range studied; interpolate Tc from nearest measured
neighbors. Absence of a sample is a synthesis gap, not a low-Tc indicator.
Both O6.160 and O6.270 interpolate to high; all O4 family measured high so
unmeasurable O4 (O4.047, O4.066, O4.160, O4.180) predicted high.
Exception note: Tl substitution (O6Cu1Ba2Hg1Tl1) collapses Tc to 0 (low) —
but that one is measurable so not a prediction target.

## Step D — among predicted-high unmeasurable, fewest elements (tie: higher expected Tc)
Predicted high & unmeasurable:
- O4.047, O4.066, O4.160, O4.180 (4 elements each)
- O6.160, O6.270 (5 elements each)
Fewest elements = 4 → the O4 members.
Tie-break higher expected Tc:
  O4.047 ~ extrapolate below 4.081(98) — expected ~95-98
  O4.066 ~ below 4.081(98), near 4.085(92) — expected ~92-98
  O4.160 ~ between 4.150(97) and 4.162(97.4) — expected ~97
  O4.180 ~ between 4.162(97.4) and 4.200(94.2) — expected ~96
O4.160 has highest expected Tc (~97.4, interpolated between two high neighbors).
Final = O4.160Cu1Ba2Hg1
