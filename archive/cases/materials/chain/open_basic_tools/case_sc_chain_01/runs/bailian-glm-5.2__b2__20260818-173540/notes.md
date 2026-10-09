# Notes: superconductor Tc study

## Task setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 26 units total (shared between measure_tc and search_db)
- "high" = Tc >= 90 K
- search_db: each page shows ≤4 results, 1 budget unit per page
- measure_tc: 1 budget unit per measurement

## DB search (Hg, 9 pages, 9 budget used, 17 left)
All 27 Hg materials:
- Group A: O4.xCu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg) x14: O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  -> Step B candidates (4-elem, Hg)
- Group B: O6.xCa1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg) x12: O6.080(seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 -> Step A series
- Group C: O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl) x1 -> oddball

## Step A measurements (O6.xCa1Cu2Ba2Hg1 series)
| O | Tc | note |
|---|---|---|
| 6.080 | 92 | seed |
| 6.100 | ? | not measured (rising slope, ~97) |
| 6.130 | 102 | |
| 6.140 | ? | not measured (rising slope) |
| 6.160 | null | UNMEASURABLE |
| 6.190 | 122 | |
| 6.210 | 127 | PEAK (tie) |
| 6.220 | 127 | PEAK (tie) |
| 6.240 | 123 | |
| 6.270 | null | UNMEASURABLE |
| 6.300 | 100 | |
| 6.310 | 98 | |

Trend: 92→102→[unme]→122→127→127→123→[unme]→100→98. Peak T* = 127.
threshold1 = 127 - 32 = 95.

## Step B measurements (O4.xCu1Ba2Hg1, 4-elem Hg set, threshold=95)
| O | Tc | Q1? |
|---|---|---|
| 4.047 | 95.0 | YES (>=95) |
| 4.066 | ERROR "unknown composition" | ??? (appeared in search page1) |
| 4.081 | 98.0 | YES |
| 4.085 | not measured | ? |
| 4.096 | not measured | ? |
| 4.097 | 95.3 | YES |
| 4.101 | not measured | ? |
| 4.103 | 92.5 | NO |
| 4.150 | 97.0 | YES |
| 4.160 | not measured | ? |
| 4.162 | 97.4 | YES |
| 4.180 | null | UNMEASURABLE |
| 4.200 | 94.2 | NO |
| 4.220 | 97.0 | YES |

Pattern observed: Tc hovers 92.5-98 across the band; no strong peak. Values near threshold 95 so borderline members matter.

## Final O4.101 result & budget
- O4.101 = 93.4 (< 95, NOT Q1)
- Budget now 26/26 EXHAUSTED.

## Final Group A table (O4.xCu1Ba2Hg1, 4-elem Hg; threshold=95)
| O | Tc | Q1(>=95)? |
|---|---|---|
| 4.047 | 95.0 | YES |
| 4.066 | error "unknown composition" | ambiguous - exclude from Q1; not a clean "no sample" signal so exclude from predictions too |
| 4.081 | 98.0 | YES |
| 4.085 | unmeasured (budget) | est ~96.6 - not in Q1 (no measured Tc) |
| 4.096 | unmeasured (budget) | est ~95.5 - not in Q1 |
| 4.097 | 95.3 | YES |
| 4.101 | 93.4 | NO |
| 4.103 | 92.5 | NO |
| 4.150 | 97.0 | YES |
| 4.160 | unmeasured (budget) | est ~97.3 - not in Q1 |
| 4.162 | 97.4 | YES |
| 4.180 | null (no sample) | UNMEASURABLE -> predictions, est ~95.8 -> HIGH |
| 4.200 | 94.2 | NO |
| 4.220 | 97.0 | YES |

## Q1 (qualifying) = 4-elem Hg materials with DIRECTLY-MEASURED Tc >= 95:
O4.047Cu1Ba2Hg1 (95.0), O4.081Cu1Ba2Hg1 (98.0), O4.097Cu1Ba2Hg1 (95.3), O4.150Cu1Ba2Hg1 (97.0), O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0).
(Estimated-but-unmeasured members 4.085~97.3, 4.096~95.5, 4.160~97.3 look >=95 too but not confirmed -> not in Q1, kept honest. O4.180 unmeasurable -> predictions. O4.066 ambiguous error -> excluded. 4.101=93.4, 4.103=92.5, 4.200=94.2 measured <95 -> excluded.)

## Note on O4.066
O4.066Cu1Ba2Hg1 appeared in search_db page 1 but measure_tc returned "unknown composition; discover it via search_db first" - NOT the canonical "No sample exists" message. Per task's own definition (unmeasurable = measure returns "no sample exists"), O4.066 is NOT classified as unmeasurable, so excluded from predictions & Q1. Its Tc is indeterminate.

## Step D resolution
Predicted-high unmeasurable = {O6.160Ca1Cu2Ba2Hg1 (5 elem), O6.270Ca1Cu2Ba2Hg1 (5 elem), O4.180Cu1Ba2Hg1 (4 elem)}. Fewest elements = 4 -> O4.180Cu1Ba2Hg1. No tie. FINAL = O4.180Cu1Ba2Hg1.

## Step C - unmeasurable materials discovered (clean "no sample exists"):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, Ca family) - O6.160 between 102@6.130 and 122@6.190 -> ~112 -> HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, Ca family) - between 123@6.240 and 100@6.300 -> ~111 -> HIGH
3. O4.180Cu1Ba2Hg1 (4 elem, Hg-1201 family) - between 97.4@4.162 and 94.2@4.200 -> ~95.8 -> HIGH (>=90)
(O4.066 returned an inconsistent "unknown composition" error - NOT a clean no-sample signal, so excluded from predictions to stay honest.)

## Rule (multi-factor)
HIGH iff: Hg-based cuprate AND has Ba-Cu-O layered framework AND oxygen within the observed high-Tc doping window of its family:
- Ca-1223 family (O,Ca,Cu,Ba,Hg; O~6.08-6.31): all measured points 92-127, so window is broad -> HIGH
- Hg-1201 family (O,Cu,Ba,Hg; O~4.05-4.22): all measured points 92.5-98 -> HIGH
Prediction uses family + framework + oxygen-in-window (not a single factor like "contains Hg" or "oxygen value alone").

## Step D
Predicted-high unmeasurable: O6.160(5e), O6.270(5e), O4.180(4e).
Fewest elements = 4 -> O4.180Cu1Ba2Hg1. FINAL = O4.180Cu1Ba2Hg1.

## FINAL ANSWER VALUES
- peak = O6.210Ca1Cu2Ba2Hg1
- peak_tc = 127
- threshold = 95
- qualifying = [O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]
- predictions = {O6.160Ca1Cu2Ba2Hg1: high, O6.270Ca1Cu2Ba2Hg1: high, O4.180Cu1Ba2Hg1: high}
- final = O4.180Cu1Ba2Hg1
- rule = multi-factor rule described above

## Step A
T* = 127 (measured at O6.210 and O6.220, tie). peak = O6.210Ca1Cu2Ba2Hg1.
threshold1 = 127 - 32 = 95.

## Plan
- Step A: find series members (same element set {O,Ca,Cu,Ba,Hg}, varying oxygen). Seed has O6.080. Need to search and measure to find max Tc = T*.
- Step B: threshold1 = T* - 32. Among materials containing Hg AND exactly 4 elements, find ALL with Tc >= threshold1. Set Q1.
- Step C: predict high/low for unmeasurable materials (those returning "no sample exists"). Need a multi-factor rule.
- Step D: among predicted-high unmeasurable, pick fewest elements (tie-break higher expected Tc).
