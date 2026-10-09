# Superconductor Tc study — notes

## Task summary
- Budget: 34 units total (shared by measure_tc and search_db; 1 unit each)
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K
- Step A: find peak Tc (=T*) of homologous doping series (O,Ca,Cu,Ba,Hg; O varies)
- Step B: threshold1 = T* - 32; Q1 = all materials w/ Hg and exactly 4 elements with Tc >= threshold1
- Step C: find unmeasurable materials, predict high/low via induced rule (multi-factor)
- Step D: among predicted-high unmeasurable, pick fewest elements (tiebreak higher expected Tc)

## Budget tracker
- Start: 34
- Used: 29
- Remaining: 5 (done measuring; all landscape measured)

## Step B — 4-element Hg materials, threshold1 = 95.0 (Q1 = Tc >= 95)
| composition | Tc | Q1? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | UNMEASURABLE | predict |
| O4.066Cu1Ba2Hg1 | UNMEASURABLE | predict |
| O4.081Cu1Ba2Hg1 | 98.0 | ✅ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✅ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✅ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✅ |
| O4.160Cu1Ba2Hg1 | UNMEASURABLE | predict |
| O4.162Cu1Ba2Hg1 | 97.4 | ✅ |
| O4.180Cu1Ba2Hg1 | UNMEASURABLE | predict |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✅ |

Q1 (confirmed measurable, Tc >= 95.0): O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1
(Note: unmeasurable materials cannot be confirmed Q1 members by measurement; they go to Step C predictions.)

## O6Cu1Ba2Hg1Tl1 (5-elem, not a Q1 candidate — has 5 elements)
- Tc = 0.0 (measurable but not superconducting / below threshold)

## Step C — unmeasurable materials discovered (must predict all)
1. O6.160Ca1Cu2Ba2Hg1 (series, 5 elem) — O6.160
2. O6.270Ca1Cu2Ba2Hg1 (series, 5 elem) — O6.270
3. O4.047Cu1Ba2Hg1 (4 elem)
4. O4.066Cu1Ba2Hg1 (4 elem)
5. O4.160Cu1Ba2Hg1 (4 elem)
6. O4.180Cu1Ba2Hg1 (4 elem)

## Rule (multi-factor, physics-grounded)
Predict HIGH iff BOTH hold:
(1) cation framework matches a measured-high Hg-cuprate family with same CuO2-layer count:
    - Cu1Ba2Hg1 + O4.x = 1 CuO2 layer (Hg-1201 analog), measured all high (92–98 K)
    - Ca1Cu2Ba2Hg1 + O6.x = 2 CuO2 layers (Hg-1212 analog), measured all high (92–127 K)
    This excludes unrelated Hg phases (e.g. O6Cu1Ba2Hg1Tl1 measured 0.0).
(2) oxygen content lies WITHIN the measured high-Tc window of that family (interpolation),
    not extrapolated past the confirmed range.

Measured windows:
- 4E (Cu1): O ∈ [0.081, 0.220] all high
- 5E (Cu2): O6.x ∈ [0.080, 0.310] all high (dome peak ~127 @ 0.21)

## Predictions
- O4.047Cu1Ba2Hg1 → LOW  (O=0.047 < 0.081, extrapolated underdoped edge; steep 98→92 drop at 0.081→0.085 implies further underdoping → below 90)
- O4.066Cu1Ba2Hg1 → LOW  (O=0.066 < 0.081, same reasoning)
- O4.160Cu1Ba2Hg1 → HIGH (O=0.160 within [0.081,0.220]; ~97.3 by interp 0.150/0.162)
- O4.180Cu1Ba2Hg1 → HIGH (O=0.180 within window; ~95.9 by interp 0.162/0.200)
- O6.160Ca1Cu2Ba2Hg1 → HIGH (O6.160 within [0.080,0.310]; ~113 between 0.140@105 & 0.190@122)
- O6.270Ca1Cu2Ba2Hg1 → HIGH (O6.270 within window; ~111 between 0.240@123 & 0.300@100)

Why single-factor rules fail: "all Hg cuprates high" wrongly marks O4.047/O4.066 high
(they're underdoped, below the measured window). "O value alone" ignores structure
family (would miss that O6Cu1Ba2Hg1Tl1, a different framework, measured 0.0).

## Step D — final pick
Predicted-HIGH unmeasurables: O4.160(4e), O4.180(4e), O6.160(5e), O6.270(5e)
Fewest elements → 4-element tie: O4.160 vs O4.180
Tiebreak higher expected Tc: O4.160 ~97.3 > O4.180 ~95.9
FINAL = O4.160Cu1Ba2Hg1

## Step A peak
T* = 127.0 at O6.210Ca1Cu2Ba2Hg1 (first occurrence of max; O6.220 also 127.0 tie)
threshold1 = 127.0 − 32 = 95.0

## Q1 (Step B, measured & confirmed Tc >= 95.0, Hg + exactly 4 elements)
O4.081Cu1Ba2Hg1 (98.0), O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3),
O4.150Cu1Ba2Hg1 (97.0), O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0)
(Unmeasurable 4E materials are NOT in Q1 — their Tc is unknown, handled by Step C predictions.)

## Budget final
Used 29/34 (8 search + 21 measure). 5 unused.


## Step A — series Tc values (O,Ca,Cu,Ba,Hg)
| composition | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | UNMEASURABLE (no sample) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0 ← T* (max)** |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | UNMEASURABLE (no sample) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

- T* = 127.0, peak composition = O6.210Ca1Cu2Ba2Hg1
- threshold1 = T* - 32 = 127 - 32 = 95.0

## Unmeasurable discovered so far (Step C candidates)
- O6.160Ca1Cu2Ba2Hg1 (series, 5 elem)
- O6.270Ca1Cu2Ba2Hg1 (series, 5 elem)


## All Hg-containing materials (27 total, from "Hg" search pages 1-7)

### 4-element materials (O,Cu,Ba,Hg) — candidates for Step B Q1
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

### 5-element series (O,Ca,Cu,Ba,Hg) — Step A series, 12 members
15. O6.080Ca1Cu2Ba2Hg1 (seed)
16. O6.100Ca1Cu2Ba2Hg1
17. O6.130Ca1Cu2Ba2Hg1
18. O6.140Ca1Cu2Ba2Hg1
19. O6.160Ca1Cu2Ba2Hg1
20. O6.190Ca1Cu2Ba2Hg1
21. O6.210Ca1Cu2Ba2Hg1
22. O6.220Ca1Cu2Ba2Hg1
23. O6.240Ca1Cu2Ba2Hg1
24. O6.270Ca1Cu2Ba2Hg1
25. O6.300Ca1Cu2Ba2Hg1
26. O6.310Ca1Cu2Ba2Hg1

### Other (5-element: O,Cu,Ba,Hg,Tl)
27. O6Cu1Ba2Hg1Tl1

## Step B filter: contains Hg AND exactly 4 elements
- The 14 "O4.xCu1Ba2Hg1" materials are exactly 4 elements (O,Cu,Ba,Hg).
- The series (5 elem) and O6Cu1Ba2Hg1Tl1 (5 elem) are NOT in Q1.
- Q1 candidates = the 14 above, filtered by Tc >= threshold1 (= T* - 32).

1. Search "Ca1Cu2Ba2Hg1" to find series members (vary O)
2. Measure all series members → find T*
3. Search "Hg" paginated → find all Hg-containing materials
4. Among Hg + 4-element materials, measure those possibly above threshold1=T*-32 → Q1
5. Find unmeasurable materials (measure returns "no sample exists") → predict
6. Pick final
