# Notes: find the best unexplored superconductor candidate

## Task recap
- Budget: 45 `get_property` calls.
- `measurable.txt` = 30 materials I can measure (build model from these).
- `unmeasurable.txt` = 10 target materials; their Tc "not obtainable by measurement", must infer.
- **Goal (final)**: among unmeasurable, the single best candidate = fewest elements among those whose Tc is HIGH (Tc >= 90 K). Ties → higher expected Tc.
- **Q1**: all materials that contain Hg AND have exactly 4 elements AND Tc >= threshold1, where threshold1 = T* - 32, T* = max Tc in seed's homologous doping series.
- Seed = `O6.080Ca1Cu2Ba2Hg1`; series = same element set (Ca,Cu,Ba,Hg) varying O.

## Data inventory

### measurable.txt (30)
- Lines 1-12: Hg-1212 series `O{6.08..6.31}Ca1Cu2Ba2Hg1` (Ca,Cu,Ba,Hg + O) = 5 elements, 2 CuO2 planes.
- Lines 13-24: Hg-1201 series `O{4.047..4.220}Cu1Ba2Hg1` (Cu,Ba,Hg + O) = 4 elements, 1 CuO2 plane.
- L25: `O6Cu1Ba2Tl1` — Tl-1201, 4 elts (Cu,Ba,Tl,O).
- L26: `O10Ca2Cu3Ba2Tl2` — Tl-1223, 5 elts, 3 CuO2 planes.
- L27: `O6Cu1Cd0.250Ba2Tl1.750` — Tl/Cd-1201, 5 elts, 1 plane.
- L28: `O7Cu3Ba2La1` — La-123, 4 elts (Cu,Ba,La,O), 3 planes.
- L29: `O7Cu3Y0.200Ba2La0.800` — Y/La-123, 5 elts.

### unmeasurable.txt (10) — element counts
1. `O6.160Ca1Cu2Ba2Hg1` — 5 elts (Ca,Cu,Ba,Hg,O) [Hg-1212, in seed series!]
2. `O6.270Ca1Cu2Ba2Hg1` — 5 elts [Hg-1212, in seed series!]
3. `O4.180Cu1Ba2Hg1` — 4 elts (Cu,Ba,Hg,O) [Hg-1201, in measurable series!]
4. `O4.047Cu1Ba2Hg1` — 4 elts [Hg-1201, in measurable series!]
5. `O4.066Cu1Ba2Hg1` — 4 elts [Hg-1201, NOT in measurable; between 4.047 & 4.081]
6. `O4.160Cu1Ba2Hg1` — 4 elts [Hg-1201, NOT in measurable; between 4.150 & 4.162]
7. `O8Ca0.850Cu2.200Ba2Tl1.800` — 5 elts (Ca,Cu,Ba,Tl,O) [Tl-1212-ish, 2 planes]
8. `O7Ca1Cu2Sr2Tl0.500Pb0.500` — 6 elts (Ca,Cu,Sr,Tl,Pb,O) [2 planes, Sr-based]
9. `O8Cu3Y1Ba1.900La0.100` — 5 elts (Cu,Y,Ba,La,O) [Y/La-123, 3 Cu]
10. `O8Cu4Y1Ba1.870La0.130` — 5 elts (Cu,Y,Ba,La,O) [Cu4 variant]

## Strategy
- Fewest elements among HIGH. 4-elt unmeasurables = #3,#4,#5,#6 (all Hg-1201). If any is HIGH → final is a 4-elt Hg-1201 (fewest=4). Else fall to 5-elt high ones.
- KEY empirical question: does Hg-1201 peak reach >=90?
- Need: seed series (T* + #1,#2 values); Hg-1201 series (#3,#4 direct; #5,#6 interpolate); 5 single-point anchors for rule on #7-#10.

## Tool behavior test
get_property(identifier, prop) — prop ∈ {tc, element_count}. Compose-keyed. Will confirm whether overlapping unmeasurable strings (e.g. #1 O6.160) return value or "no measurement exists".

## Measurements log

### Probe (budget 0→2)
- `O6.080Ca1Cu2Ba2Hg1` tc = **92.0** (seed is HIGH itself; good anchor for series).
- `O6.160Ca1Cu2Ba2Hg1` tc = **null** ("never been probed") — overlap composition refused. So #1/#2 cannot be measured even though listed in measurable.txt; the unmeasurable flag is keyed on exact composition string. They must be interpolated from series.
- `O6.080Ca1Cu2Ba2Hg1` element_count = **5** (confirms Ca+Cu+Ba+Hg+O = 5 elts). Tool format = plain int.

### Seed series Hg-1212 (O{6.08..6.31}Ca1Cu2Ba2Hg1) — find T*
Already: O6.080=92.0; O6.160=null(#1); O6.270=null(#2). Measured the other 9:
- O6.080=92.0, O6.100=92.5, O6.130=102.0, O6.140=105.0, [O6.160=null], O6.190=122.0,
  O6.210=**127.0**, O6.220=**127.0**, O6.240=123.0, [O6.270=null], O6.300=100.0, O6.310=98.0
- **T\* = 127.0** (plateau at O6.21–0.22). → **threshold1 = 127 − 32 = 95.0**
- Interp #1 O6.160 (betw 0.140→105, 0.190→122): 105 + 17·(0.020/0.050) = 105 + 6.8 = **111.8** → HIGH.
- Interp #2 O6.270 (betw 0.240→123, 0.300→100): 123 − 23·(0.030/0.060) = 123 − 11.5 = **111.5** → HIGH.

### Hg-1201 series (O{4.04..4.22}Cu1Ba2Hg1) — direct for #3/#4, interp #5/#6
12 in measurable; #3(O4.180) & #4(O4.047) listed; #5(O4.066) & #6(O4.160) NOT listed → interp.
Measured all 12 (budget 11→21):
- #4 O4.047=**null** (refused — confirms overlap refusal is exact-string). Neighbors: O4.081=98.
- O4.085=92.0, O4.096=95.6, O4.097=95.3, O4.101=93.4, O4.103=92.5,
  O4.150=97.0, O4.162=97.4, #3 O4.180=**null**, O4.200=94.2, O4.220=97.0
- Hg-1201 peak region ≈ 92–98 K (top ~98 at O4.081; secondary rise to 97.4 at O4.162). **All measured points ≥ 92 K → all HIGH.**
- #5 O4.066 — below all measured (nearest O4.081=98). Extrapolate ≈ 98–99 (rising toward low-O end). **HIGH.**
- #3 O4.180 — neighbors O4.162=97.4 (Δ+0.018) & O4.200=94.2 (Δ−0.020); mid-interp 97.4 + (94.2−97.4)·(0.018/0.038) = 97.4 − 1.5 = **95.9** → HIGH (≥95 threshold).
- #6 O4.160 — betw O4.150=97.0 & O4.162=97.4 → **≈97.2** → HIGH.

So **#3, #4, #5, #6 all HIGH, all 4 elements.** Fewest-elt HIGH candidates among unmeasurables → all four tie at 4 elts.

### 5-elt structural anchors (measurable L25–29) — rule for #7–#10 (budget 21→25)
- L25 `O6Cu1Ba2Tl1` → **"unknown material"** (not even null — material not registered; element_count also errors). No Tl-1201 1-plane Ba anchor.
- L26 `O10Ca2Cu3Ba2Tl2` (Tl-1223, 3 planes) = **118.97** → HIGH.
- L27 `O6Cu1Cd0.250Ba2Tl1.750` (Tl/Cd-1201, 1 plane, Ba, 5 elts) = **88.0** → LOW.
- L28 `O7Cu3Ba2La1` (La-123, 3 planes, no Y, 4 elts) = **89.6** → LOW (just below 90).
- L29 `O7Cu3Y0.200Ba2La0.800` (Y/La-123, 3 planes, 5 elts, O7) = **81.0** → LOW.

### Induced rule (interaction, not one-factor)
One-factor rules the task warns against: "contains Hg" (Tl-1223=118.97 is HIGH w/o Hg; La-123=89.6 LOW w/ 3 Cu), "has 5 elements" (O4.081 Hg-1201 has 4 elts & HIGH=98; La-123 4-elt=89.6 LOW), "contains Ba" (most do). All fail.
Observed driver = **# CuO2 planes × doping regime**, element-count irrelevant:
- n=1 plane, single-layer 1201 (Hg or Tl/Cd): Tc ~88–98 (Hg-1201 ~92–98 HIGH; Tl/Cd-1201=88 LOW).
- n=2 planes, 1212 (Hg-1212 ~92–127; Tl-1212 unknown but Hg-1212 peaks 127): HIGH.
- n=3 planes, 1223 (Tl-1223=118.97): HIGH.
- La/Y-123 (3 Cu but the Cu is *in the perovskite 123 stoichiometry* O7, underdoped at O7 not O~6.9): ~81–89.6 → LOW (these are the YBCO-123 family at O7, oxygen-underdoped relative to the ~90 K optimum at O6.9).

### Classification of all 10 unmeasurables
Hg-1201 (4 elts, all HIGH ≈92–99): #3≈95.9, #4≈98–99, #5≈98.3, #6≈97.2.
Hg-1212 (5 elts, HIGH): #1≈111.8, #2≈111.5.
Tl-1212 #7 (5 elts): 2-plane Tl analog of Hg-1212 (peaks 127) & Tl-1223=118.97 → **HIGH** (~95–118).
Tl/Pb-1212-Sr #8 (6 elts): 2 planes, Sr-based, Pb/Tl mixed. Hg-1212-Sr series (Cu2Ba2SrxCa1-x) are known HIGH; but this is a *1-plane-equivalent* Tl/Pb 1201-style charge reservoir with Sr, borderline. With only Tl/Cd-1201=88 (LOW) and Tl-1223=118.97 (HIGH) as anchors and no 1212-Sr direct data, **classification uncertain → conservatively LOW** (cannot confirm ≥90).
Y/La-123-Cu3 #9 & -Cu4 #10 (5 elts): same YBCO-123 family as L28/L29 (89.6/81.0, both LOW), at O8 (over-oxygenated, still 123-stoichiometry perovskite, not optimally doped) → **LOW**.

### Goal: fewest elements among HIGH
- 4 elts HIGH: #3, #4, #5, #6 (Hg-1201). ← fewest = 4.
- 5 elts HIGH: #1, #2, #7.
→ Winner among the 4-elt HIGH set, tie broken by higher expected Tc.
Expected Tc ranking (from Hg-1201 curve, peak O4.081=98.0):
  #4 (O4.047, just below peak, rising edge) ≈ 98–99  ← highest
  #5 (O4.066) ≈ 98.3
  #6 (O4.160, near O4.162=97.4) ≈ 97.2
  #3 (O4.180) ≈ 95.9
→ **FINAL = `O4.047Cu1Ba2Hg1`** (4 elements, HIGH, highest expected Tc ≈ 98–99).

### Q1: Hg AND 4 elts AND Tc ≥ threshold1=95.0
Candidates (Hg & 4-elt among unmeasurable) = #3,#4,#5,#6. Expected Tc: #3≈95.9✓, #4≈98–99✓, #5≈98.3✓, #6≈97.2✓. All ≥ 95.
→ **Q1 = {O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1}**

### Budget summary
Used 25 tc + element_count calls charged (element_count "free from formula" did not charge). 45 − 25 = 20 unused. Measurement count is conservative and well within budget.

### Tiebreak verification (#4 vs #5, both ≈98)
Probed O4.060/O4.070/O4.050 → all "unknown material". Confirms only the 30 listed compositions are registered; cannot map the low-O Hg-1201 peak directly.
Rising-edge argument from {O4.081=98.0, O4.085=92.0}: Tc rises steeply as O decreases below 0.081. #5 (O4.066, 0.015 below peak) ≈ 98.3; #4 (O4.047, 0.034 below peak) ≥ 98.3, plausibly ~99 → **#4 has highest expected Tc**. FINAL = `O4.047Cu1Ba2Hg1`.

### Final deliverable cross-check
- `peak` = O6.210Ca1Cu2Ba2Hg1 (T*=127.0, ties with O6.220=127.0; pick lower O). `peak_tc`=127.0.
- `threshold` = 127−32 = 95.0.
- `qualifying` (Q1: Hg ∧ 4-elt ∧ Tc≥95) = #4,#5,#6,#3 (all ≈95.9–99 ≥ 95).
- `predictions`: 10 entries, one per unmeasurable line.
- `final` = O4.047Cu1Ba2Hg1 (4 elts, HIGH, highest expected Tc among the 4-elt HIGH tie).
- High: #1,#2,#3,#4,#5,#6,#7 (7). Low: #8,#9,#10 (3).
