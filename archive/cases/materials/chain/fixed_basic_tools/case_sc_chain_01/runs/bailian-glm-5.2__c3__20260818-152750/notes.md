# Superconductor Tc study — running notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family, HgBa2CaCu2O6+δ)
- HIGH = Tc >= 90 K, LOW otherwise.
- Budget: 18 `measure_tc` calls.

## Data inventory
measurable.txt (26 entries):
- Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 series (Hg-1212, Cu2, varying O) — the seed's homologous series (Step A).
- Lines 11-21: O4.xxxCu1Ba2Hg1 series (Hg-1201, Cu1, varying O) — Hg + exactly 4 elements → Step B candidates (11 members).
- Line 22: O6Cu1Ba2Hg1Tl1 (Hg+Tl 1201, Cu1, 5 el)
- Line 23: O10Ca2Cu3Ba2Tl2 (Tl-2223, Cu3)
- Line 24: O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 Cd-doped, Cu1)
- Line 25: O7Cu3Ba2La1 (La-123, Cu3)
- Line 26: O7Cu3Y0.200Ba2La0.800 (Y/La-123, Cu3)

unmeasurable.txt (7 entries):
1. O6.160Ca1Cu2Ba2Hg1  — Hg-1212 Cu2, O6.16 (near optimal ~6.1)
2. O6.270Ca1Cu2Ba2Hg1  — Hg-1212 Cu2, O6.27 (overdoped)
3. O4.180Cu1Ba2Hg1     — Hg-1201 Cu1, O4.18 (overdoped)
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-1212 Cu2, O8 (overdoped?)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — (Tl,Pb)-1212 Sr-variant Cu2, O7 (optimal?)
6. O8Cu3Y1Ba1.900La0.130 — Y-123 Cu3, O8 (overdoped?)
7. O8Cu4Y1Ba1.870La0.130 — Y-124 Cu4, O8 (inherent ~80K)

## Budget plan (18 calls)
- Step A (8): O6 series 6.080, 6.100, 6.130, 6.140, 6.190, 6.210, 6.220, 6.240.
  Skip 6.300, 6.310 (most overdoped, cannot be the peak). Find T* = max.
- Step B (7): O4 series 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150.
  Skip 4.047 (underdoped), 4.200, 4.220 (extreme overdoped) — clearly below threshold.
  Q1 = members with Tc >= T*-32.
- Step C (3): extras for rule diversity —
  line 23 O10Ca2Cu3Ba2Tl2 (Tl-2223 Cu3),
  line 24 O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 Cu1),
  line 26 O7Cu3Y0.200Ba2La0.800 (Y/La-123 Cu3).

## Step A results (Hg-1212, O6 series) — budget 8/18
| O6.xxx | 080 | 100 | 130 | 140 | 190 | 210 | 220 | 240 |
| Tc (K) | 92.0 | 92.5 | 102.0 | 105.0 | 122.0 | 127.0 | 127.0 | 123.0 |
- Bell curve: peak at O6.210 & O6.220 (flat top 127). 6.240 already declining (123).
- Skipped 6.300, 6.310 (overdoped, certainly < 127).
- **T* = 127.0** at O6.210Ca1Cu2Ba2Hg1 (first max; tie with O6.220).
- **threshold1 = 127 - 32 = 95.0 K.**

## Step B candidates (Hg + exactly 4 elements)
= O4.xxxCu1Ba2Hg1 series (elements O,Cu,Ba,Hg). 11 members: O4.047, 4.081, 4.085,
4.096, 4.097, 4.101, 4.103, 4.150, 4.162, 4.200, 4.220.
Need ALL with Tc >= 95.

## Plan (10 budget left)
- O4 measurements (7): 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.200.
  4.085 = low boundary of >=95 window; 4.096/4.097/4.101/4.103 = cluster near peak;
  4.150 = high boundary; 4.200 = overdoped (for #3 prediction).
  Skip 4.047/4.081/4.162/4.220 (extrapolate — surely < 95).
- Step C calibrants (3): line 23 (O10Ca2Cu3Ba2Tl2, Tl-2223 Cu3),
  line 24 (O6Cu1Cd0.250Ba2Tl1.750, Tl-1201 Cu1), line 26 (O7Cu3Y0.200Ba2La0.800, Y/La-123 Cu3 O7).

## Step B results (O4 / Hg-1201 series, threshold = 95.0) — budget 15/18
| O4.xxx | 085 | 096 | 097 | 101 | 103 | 150 | 162 | 200 |
| Tc (K) | 92.0 | 95.6 | 95.3 | 93.4 | 92.5 | 97.0 | 97.4 | 94.2 |
- ≥95.0 (in Q1): 4.096, 4.097, 4.150, 4.162.
- <95.0 (excluded): 4.085(92), 4.101(93.4), 4.103(92.5), 4.200(94.2).
- Skipped 4.047, 4.081, 4.220 — inferred < 95:
  · 4.047, 4.081: underdoped side below 4.085(92.0) → Tc < 92 < 95.
  · 4.220: overdoped past 4.200(94.2, declining) → Tc < 94.2 < 95.
- **Q1 = {O4.096, O4.097, O4.150, O4.162}Cu1Ba2Hg1** (all Hg + exactly 4 elements & ≥95).

## Step C calibrants (cross-family, budget 16-18/18)
- O10Ca2Cu3Ba2Tl2 (Tl-2223, Cu3, O10) → **118.97 K (HIGH)**.
- O7Cu3Y0.200Ba2La0.800 (Y/La-123, Cu3, O7) → **81.0 K (LOW)**.
- (3rd call used for O4.162 above.)
- Also note: line 24 O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 Cu1) NOT measured (ran out).

## Step C — induced rule (multi-factor)
Measured HIGH: Hg-1212 (Cu2, O6.08–6.24, 92–127), Hg-1201 (Cu1, O4.085–4.20, 92–97.4),
Tl-2223 (Cu3, O10, 118.97). Measured LOW: Y/La-123 (Cu3, O7, 81.0).
Rule = interaction of (1) Hg/Tl charge-reservoir cation vs Y/La/Ba-only 123/124 family;
(2) Cu count 1–3 (ok) vs ≥4 (Y-124 ~80 K); (3) oxygen in-dome vs heavily overdoped past it.
Y-123/124 family caps ~80–92 K; La-substitution or O8-overdoping pushes < 90 → LOW.

## Step C — predictions for unmeasurable.txt (7)
1. O6.160Ca1Cu2Ba2Hg1 → **high** (Hg-1212, O6.16 in measured HIGH dome 6.08–6.24).
2. O6.270Ca1Cu2Ba2Hg1 → **high** (Hg-1212, O6.27; gentle decline 6.22→6.24 is −4 K/0.02,
   extrapolate ~117 K, still well inside broad Hg-1212 dome).
3. O4.180Cu1Ba2Hg1 → **high** (Hg-1201, O4.18 between 4.162=97.4 & 4.200=94.2 → ~96 K).
4. O8Ca0.850Cu2.200Ba2Tl1.800 → **high** (Tl-2212-like, Cu2, O8 = optimal for double-Tl 2212;
   base ~105–118 K; Hg/Tl-family factor dominates over Ca/Tl deficiency).
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → **high** ((Tl,Pb)-1212, Cu2, O7 optimal, ~90–120 K).
6. O8Cu3Y1Ba1.900La0.130 → **low** (Y-123 family, O8 overdoped past O7 opt + La0.13 → <90).
7. O8Cu4Y1Ba1.870La0.130 → **low** (Y-124, Cu4, inherent ~80 K + La → <90).
HIGH = {1,2,3,4,5}; LOW = {6,7}.
Disagrees w/ "contains Hg" on #4,#5 (non-Hg HIGH); w/ "has 5 elements" on #3,#5,#6,#7.

## Step D — final pick
HIGH set = {1,2,3,4,5}. Element counts: #1=5, #2=5, #3=4 (O,Cu,Ba,Hg), #4=5, #5=6.
Fewest elements = #3 (4 elements, unique min). No tie-break needed.
**final = O4.180Cu1Ba2Hg1** (robust: #3 is clearly HIGH ~96 K & is the only 4-el unmeasurable).

## Final answer values
- peak = "O6.210Ca1Cu2Ba2Hg1" (Tc=127.0; O6.220 also 127.0 tie — pick first in file).
- peak_tc (T*) = 127.0
- threshold (threshold1) = 127.0 - 32 = 95.0
- qualifying (Q1) = ["O4.096Cu1Ba2Hg1","O4.097Cu1Ba2Hg1","O4.150Cu1Ba2Hg1","O4.162Cu1Ba2Hg1"]
- predictions: see above (all 7).
- final = "O4.180Cu1Ba2Hg1"
