# Superconductor Tc study - Notes

## Task summary
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg)
- High = Tc >= 90 K, Low = Tc < 90 K

## Steps
- A: Find series peak (same elements, varying O). Measure members, find max Tc = T*.
- B: threshold1 = T* - 32. Among materials with Hg AND exactly 4 elements, report ALL with Tc >= threshold1. Set = Q1.
- C: Predict high/low for unmeasurable materials (measure_tc returns "no sample exists"). Induce rule.
- D: Among predicted-high unmeasurable, pick FEWEST elements (tie-break higher expected Tc).

## Budget tracking
Starting budget: 45

## Search results (Hg, 27 total, 2 per page, 14 pages used = budget 17; +1 for seed exact search = 18? let me recount)
Actually: search O6.080Ca1Cu2Ba2Hg1 (1), CaCuBaHg (2), HgBaCaCu (3), Hg pages 1-13 (4-17). So 17 search units used. Then 6 measures so far on series.

## All Hg materials discovered (27):
Seed series (Ca1Cu2Ba2Hg1, 5 elements: O,Ca,Cu,Ba,Hg) — 12 members:
- O6.080 (seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310

O4 series (Cu1Ba2Hg1, 4 elements: O,Cu,Ba,Hg) — 14 members:
- O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220

5-element (O,Cu,Ba,Hg,Tl): O6Cu1Ba2Hg1Tl1

## Step A measurements (seed series):
- O6.080 → 92
- O6.190 → 122
- O6.210 → 127  <-- peak
- O6.220 → 127  <-- peak
- O6.240 → 123
- O6.310 → 98
T* = 127 (max). Peak composition: O6.210Ca1Cu2Ba2Hg1 (and O6.220 also 127).
Will report O6.210Ca1Cu2Ba2Hg1 as peak (first one at max; tie-break? task says identify composition with MAXIMUM Tc, doesn't specify tie-break. Both 127. I'll pick O6.210Ca1Cu2Ba2Hg1.)

## Step B:
threshold1 = T* - 32 = 127 - 32 = 95
Need: Hg + exactly 4 elements + Tc >= 95.
4-element Hg materials = the 14 O4-series members (Cu1Ba2Hg1 = {O,Cu,Ba,Hg}).
O4 series measurements (sorted by O):
- O4.047 → UNMEASURABLE (no sample)
- O4.066 → UNMEASURABLE
- O4.081 → 98.0  ✓(>=95)
- O4.085 → 92.0  ✗
- O4.096 → 95.6  ✓
- O4.097 → 95.3  ✓
- O4.101 → 93.4  ✗
- O4.103 → 92.5  ✗
- O4.150 → 97.0  ✓
- O4.160 → UNMEASURABLE
- O4.162 → 97.4  ✓
- O4.180 → UNMEASURABLE
- O4.200 → 94.2  ✗
- O4.220 → 97.0  ✓
Q1 (Hg + 4 elem + Tc>=95, measured) = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}
(Unmeasurable ones have no measured Tc, so not in Q1; they go to Step C.)

## KEY BUDGET INSIGHT:
- Measurable measure_tc calls cost 1 budget.
- UNMEASURABLE measure_tc calls cost 0 budget (budget_used unchanged).
- search_db costs 1/page always.

## Ca search (8 pages, budget 35→42): found 3 new Tl-family materials (rest overlap with Hg):
- O10Ca2Cu3Ba2Tl2 (5 elem: O,Ca,Cu,Ba,Tl) → 118.97 (HIGH, measurable) [Tl-2223]
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb) → UNMEASURABLE [(Tl,Pb)-Sr-1212]
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl) → UNMEASURABLE [Tl-2212/1212]

## Tl search page 1 (budget 42→45, EXHAUSTED): 5 Tl materials total, saw O10Ca2Cu3Ba2Tl2, O6Cu1Ba2Hg1Tl1 (both known). 3 more Tl materials undiscovered (pages 2-3, can't search — budget gone). Of the 4 discovered Tl materials, all measured.
O6Cu1Ba2Hg1Tl1 (5 elem) → 0.0 (LOW, measurable) [Hg/Tl-1201 over-oxygenated]

## Other Ca-family confirmed measurable (dome flank, budget-exhausted errors):
- O6.100, O6.300, O6.130 → "instrument time exhausted" = sample exists (measurable). Seed family fully measurable.

## ALL UNMEASURABLE MATERIALS DISCOVERED (6 total):
1. O4.047Cu1Ba2Hg1   (4 elem; Hg-1201, δ=0.047, heavily under-doped)
2. O4.066Cu1Ba2Hg1   (4 elem; Hg-1201, δ=0.066, under-doped, near dome onset)
3. O4.160Cu1Ba2Hg1   (4 elem; Hg-1201, δ=0.160, in dome)
4. O4.180Cu1Ba2Hg1   (4 elem; Hg-1201, δ=0.180, in dome)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  (6 elem; (Tl,Pb)-Sr-1212, O7 base matches)
6. O8Ca0.850Cu2.200Ba2Tl1.800  (5 elem; Tl-2212/1212, O8 base matches)

## MEASURED DATA SUMMARY (for rule induction):
Hg-1201 (O4, Cu1): all HIGH, Tc 92-98 (noisy, slight dome: floor ~92 at δ=0.085, peak ~98 at δ=0.081, ~97 at δ=0.15-0.22)
Hg-1212 (O6, Cu2): all HIGH, clean dome 92(O6.080)→127(O6.21)→98(O6.310)
Tl-2223 (O10, Cu3): HIGH, 119
Hg/Tl-1201 (O6, Cu1): LOW, 0.0  ← only measured LOW; over-oxygenated (Cu≈3+)

## INDUCED RULE (multi-factor):
Tc is HIGH (≥90K) iff the material is a charge-balanced cuprate in its superconducting dome, judged by TWO factors jointly:
(1) CuO2-plane count (Cu atoms) sets the Tc ceiling — 1-plane ~95K (1201), 2-plane ~127K (1212), 3-plane ~120-135K (2223);
(2) oxygen stoichiometry must place Cu near 2+ (in the dome) — neither heavily under-doped (Cu<2+, low-δ tail → low) nor over-oxidized (Cu³+, e.g. the Hg/Tl-1201 with O6 → Tc=0).
Single-factor rules fail: Cu-count alone is wrong (Cu1 Hg-1201 high but Cu1 Hg/Tl-1201 low); oxygen alone is wrong (measured Hg-1201 Tc is non-monotonic in δ).

## PREDICTIONS (Step C):
1. O4.047Cu1Ba2Hg1 → low   (heavily under-doped Hg-1201, below dome onset)
2. O4.066Cu1Ba2Hg1 → high  (under-doped but near dome onset, close to measured O4.081=98)
3. O4.160Cu1Ba2Hg1 → high  (in dome, between O4.150=97 and O4.162=97.4, ~97)
4. O4.180Cu1Ba2Hg1 → high  (in dome, between O4.162=97.4 and O4.200=94.2, ~96)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → high  ((Tl,Pb)-Sr-1212, Cu2 family consistently high, O7 base matches 1212)
6. O8Ca0.850Cu2.200Ba2Tl1.800 → high  (Tl-2212/1212, Cu2, O8 base matches, Tl-cuprates high ~80-110)

## STEP D:
Predicted-HIGH unmeasurable: #2, #3, #4 (all 4-elem), #5 (6-elem), #6 (5-elem).
Fewest elements = 4 → candidates O4.066, O4.160, O4.180.
Tie-break higher expected Tc: O4.160 (~97.3) > O4.180 (~96) > O4.066 (~93-95).
FINAL = O4.160Cu1Ba2Hg1.
(Robust: O4.160 is near the Hg-1201 dome peak ~97-98; no other 4-elem unmeasurable exceeds it; 5/6-elem ones lose on element count.)

