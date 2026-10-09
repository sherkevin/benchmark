# Superconductor Tc Study — Notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (shared: measure_tc + search_db, 1 unit each)
- High = Tc >= 90 K; low = Tc < 90 K
- search_db: 3 results/page, 1 unit/page

## Budget tracking
- Used so far: 13 (Hg p1-9 = 9, Ca1Cu2Ba2Hg1 p1-4 = 4)
- Remaining: 21
- NOTE: Hg p6-9 were partly redundant (re-found seed series). Optimal search would've been
  just "Hg" 9 pages = 9 (finds everything). I over-spent by 4. Must be careful now.

## Full material inventory (from "Hg" search, 27 total)
4-element Hg (O,Cu,Ba,Hg) — O4.xCu1Ba2Hg1 family (14): for Steps B/C/D
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
5-element Hg (seed series, 12): O6.080..O6.310 (Step A)
5-element Hg (Tl): O6Cu1Ba2Hg1Tl1 (not 4-elem, skip for Step B; maybe measure for C/D)

## Measurement plan (21 budget left)
- Step A: measure all 12 seed series → find T* (peak). [12 budget, leaves 9]
- Step B/C/D: measure 9 of 14 O4.x (strategic spread). [9 budget, leaves 0]
- Predict the 5 unmeasured O4.x via induced rule.

## KEY DISCOVERY: unmeasurable measurements are FREE
Budget math: 13 searches + 12 measurements = 25 calls, but budget_used = 23.
The 2 unmeasurable (O6.160, O6.270 → null) did NOT cost budget. Makes sense:
"no sample" = instrument didn't run. So failed measure_tc = 0 cost.
→ I can try to measure ALL 14 O4.x; only measurable ones cost. If ≤11 measurable, I'm OK.
Risk: if 12+ are measurable, I overrun. Series had 2/12 unmeasurable (~17%), so expect ~2-3.

## Step A results (measured)
| O      | Tc    |
|--------|-------|
| 6.080  | 92.0  |
| 6.100  | 92.5  |
| 6.130  | 102.0 |
| 6.140  | 105.0 |
| 6.160  | NULL (unmeasurable) ★ |
| 6.190  | 122.0 |
| 6.210  | 127.0 ← PEAK (tie) |
| 6.220  | 127.0 ← PEAK (tie) |
| 6.240  | 123.0 |
| 6.270  | NULL (unmeasurable) ★ |
| 6.300  | 100.0 |
| 6.310  | 98.0  |
T* = 127.0 at O6.210 & O6.220 (tie). Curve rises 92→127 (peak 6.21-6.22) → declines to 98.
Pick peak = O6.210Ca1Cu2Ba2Hg1 (first to reach max). threshold1 = 127 - 32 = 95.0.
Unmeasurable series members to predict: O6.160Ca1Cu2Ba2Hg1 (interp ~113, HIGH),
  O6.270Ca1Cu2Ba2Hg1 (interp ~112, HIGH).

## Step B/C/D plan (11 budget left; unmeasurable free)
- Measure O4.xCu1Ba2Hg1 family (14: O4.047..O4.220). Find Q1 = Tc >= 95.0.
- Real Hg-1201 analog: peak Tc ~97K near optimal O. Expect middle O4.x (~0.15-0.18) in Q1.
- Discover unmeasurable O4.x (free). Induce rule. Predict ALL unmeasurable found.
- Step D: among predicted-HIGH unmeasurable, pick FEWEST elements
  (O4.x have 4 elem; O6.16/6.27 have 5 elem → O4.x wins if any predicted high).
- Search O4.x (14): O4.047, 0.066, 0.081, 0.085, 0.096, 0.097, 0.101, 0.103,
    0.150, 0.160, 0.162, 0.180, 0.200, 0.220.

## Step B/C/D measurements so far (budget 30/34, 4 left)
| O4.x  | Tc    | Q1 (>=95)? | Status |
|-------|-------|-----------|--------|
| 0.047 | NULL  | ?         | UNMEASURABLE ★ |
| 0.066 | ?     | ?         | unmeasured |
| 0.081 | ?     | ?         | unmeasured |
| 0.085 | 92.0  | no        | measured |
| 0.096 | ?     | ?         | unmeasured |
| 0.097 | 95.3  | YES ✓     | measured |
| 0.101 | ?     | ?         | unmeasured |
| 0.103 | 92.5  | no        | measured |
| 0.150 | 97.0  | YES ✓     | measured |
| 0.160 | NULL  | ?         | UNMEASURABLE ★ |
| 0.162 | 97.4  | YES ✓ (max)| measured |
| 0.180 | NULL  | ?         | UNMEASURABLE ★ |
| 0.200 | 94.2  | no        | measured |
| 0.220 | 97.0  | YES ✓     | measured |
Unmeasurable found: 0.047, 0.160, 0.180 (3 of 9 measured so far; ~33%).
Unmeasured O4.x: 0.066, 0.081, 0.096, 0.101 (4 unknown).
Curve so far: 0.085=92, 0.097=95.3, 0.103=92.5, 0.150=97, 0.162=97.4(max),
  0.200=94.2(dip), 0.220=97. Looks dome-ish but noisy; 0.097 is a local peak.

## Plan for last 4 measurements
Need to (a) not miss any Q1 member, (b) find unmeasurable ones for Step C/D.
4 unknown unmeasured: 0.066, 0.081, 0.096, 0.101.
Measure all 4 → resolve Q1 gaps AND discover more unmeasurable (free).
Spend last 4 on 0.066, 0.081, 0.096, 0.101. Exactly exhausts budget.
Known unmeasurable so far: O6.160 & O6.270 (Step A) + O4.047, O4.160, O4.180 (Step B).
If more found among 0.066/0.081/0.096/0.101 → more predictions for Step C/D.

## FINAL data — all measured

### Step A series O6.xCa1Cu2Ba2Hg1 (12; 2 unmeasurable)
| O6.x  | Tc    |
|-------|-------|
| 0.080 | 92.0  |
| 0.100 | 92.5  |
| 0.130 | 102.0 |
| 0.140 | 105.0 |
| 0.160 | NULL★ |
| 0.190 | 122.0 |
| 0.210 | 127.0 ← T* (tie) |
| 0.220 | 127.0 ← T* (tie) |
| 0.240 | 123.0 |
| 0.270 | NULL★ |
| 0.300 | 100.0 |
| 0.310 | 98.0  |
T* = 127.0. Peak chosen = O6.210Ca1Cu2Ba2Hg1 (first max). threshold1 = 95.0.

### Step B/C/D family O4.xCu1Ba2Hg1 (14; 4 unmeasurable)
| O4.x  | Tc    | >=95? | Unmeas? |
|-------|-------|-------|---------|
| 0.047 | NULL  |  -    | YES ★ |
| 0.066 | NULL  |  -    | YES ★ |
| 0.081 | 98.0  | YES✓  |  |
| 0.085 | 92.0  | no   |  |
| 0.096 | 95.6  | YES✓ |  |
| 0.097 | 95.3  | YES✓ |  |
| 0.101 | 93.4  | no   |  |
| 0.103 | 92.5  | no   |  |
| 0.150 | 97.0  | YES✓ |  |
| 0.160 | NULL  |  -   | YES ★ |
| 0.162 | 97.4  | YES✓ |  |
| 0.180 | NULL  |  -   | YES ★ |
| 0.200 | 94.2  | no   |  |
| 0.220 | 97.0  | YES✓ |  |

## Step B — Q1 (Hg & exactly 4 elements & Tc >= 95.0)
Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1  (6 members)
(Ca1Cu2Ba2Hg1 series has 5 elements → excluded; O6Cu1Ba2Hg1Tl1 has 5 → excluded.)

## Step C — predict unmeasurable materials (ALL discovered via search_db)
All unmeasurable found via search_db:
  A-series (5 elem): O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
  B-family (4 elem): O4.047, O4.066, O4.160, O4.180 (Cu1Ba2Hg1)
Total = 6 unmeasurable to predict.

### Rule induction (warning: single-factor wrong on several)
Step A series = classic doping dome: low(92)→rises→peak 127 @0.21-0.22→falls→98 @0.31.
Curve is smooth; unmeasurable 6.16 (~interior rising, ~113K → HIGH), 6.27 (~112K → HIGH).
Step B curve: 92,92.5,93.4,95.3,95.6,98,97,97.4,97,97,94.2,97 — also dome-shaped,
with Tc rising into the optimal window then declining. LOW at extremes (overdoped/underdoped).

Multi-factor rule (induced from both series): **Hg cuprate doping-dome rule** —
Tc is HIGH (>=90, and for Q1 >=95) when oxygen content is WITHIN the superconducting
doping window; LOW when outside (heavily underdoped or overdoped, i.e. far from peak).
Specifically for Hg cuprates: superconducting window spans roughly O4.08–O4.22 (O4 family)
and O6.10–O6.30 (O6 family); outside → low.

### Apply to A-series unmeasurable
O6.160: between 6.140(105,HIGH) & 6.190(122,HIGH) → interior, rising → HIGH (predicted ~115).
O6.270: between 6.240(123,HIGH) & 6.300(100,HIGH) → interior, declining → still HIGH (pred ~110).

### Apply to B-family unmeasurable (4-elem O4.xCu1Ba2Hg1)
Measured bracket: 6.27 pred HIGH (interior), 6.16 pred HIGH (interior); LOW would be far extremes.
O4.047: lowest O (heavy underdope). Below lowest measured (0.066 NULL→ infer low end).
  Far underdoped, outside window → LOW.
O4.066: very low O (heavy underdope). Boundary. By window [0.08–0.22], 0.066 < 0.08 → LOW.
O4.160: between 0.150(97,HIGH) & 0.162(97.4,HIGH) → right at peak, interior → HIGH.
O4.180: between 0.162(97.4,HIGH) & 0.200(94.2,just below 95 but still HIGH>=90) & 0.220(97,HIGH)
  → interior, near peak → HIGH.

## Step D — final pick
Unmeasurable predicted HIGH: O6.160(5e), O6.270(5e), O4.160(4e), O4.180(4e).
Choose FEWEST elements → 4-element O4.160Cu1Ba2Hg1 & O4.180Cu1Ba2Hg1 tie.
Break tie by higher expected Tc: O4.160 sits between 0.150(97.0) & 0.162(97.4) → ~97.2.
O4.180 between 0.162(97.4) & 0.200(94.2), near 0.220(97.0) → ~96. O4.160 slightly higher.
FINAL = O4.160Cu1Ba2Hg1.

## Induced rule (multi-factor; why single-factor rules fail)
Hg-based cuprates: Tc = f(number of Cu-O layers, oxygen doping). Both factors needed.
- Layer count sets PEAK Tc: 2 Cu layers (Ca1Cu2, Hg-1212-like) → peak ~127 K;
  1 Cu layer (Cu1, Hg-1201-like) → peak ~97 K.
- Doping must be near-optimal; heavy under/over-doping → Tc collapses below 90 (LOW).
- Single-factor rules that FAIL:
  (a) "layer count only" → wrongly predicts heavily-underdoped 1-Cu O4.047/O4.066 as HIGH
      (they're LOW: measured O4.081=98 → O4.085=92, steep underdoped drop ⇒ below 90 by 0.066/0.047).
  (b) "oxygen range only" → would mislabel interior 2-Cu materials and ignore that
      2-Cu materials stay HIGH across a wide doping span.
- Predictions (high = Tc>=90):
  O6.160Ca1Cu2Ba2Hg1 (2Cu, interior 6.14-6.19, ~115) → HIGH
  O6.270Ca1Cu2Ba2Hg1 (2Cu, interior 6.24-6.30, ~110) → HIGH
  O4.047Cu1Ba2Hg1 (1Cu, heavy underdope, extrapolate ~60) → LOW
  O4.066Cu1Ba2Hg1 (1Cu, heavy underdope, extrapolate ~75) → LOW
  O4.160Cu1Ba2Hg1 (1Cu, at peak 0.150-0.162, ~97.3) → HIGH
  O4.180Cu1Ba2Hg1 (1Cu, interior 0.162-0.200, ~96) → HIGH

## Final answers
- peak (Step A) = O6.210Ca1Cu2Ba2Hg1 (first to reach T*=127.0; tie with O6.220)
- peak_tc = 127.0
- threshold1 = 127.0 - 32 = 95.0
- Q1 (4-elem Hg, measured Tc>=95.0) = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1
- predictions (6 unmeasurable): O6.160→high, O6.270→high, O4.047→low, O4.066→low,
  O4.160→high, O4.180→high
- final (Step D) = O4.160Cu1Ba2Hg1 (4 elem; among predicted-high unmeasurable, fewest
  elements beats 5-elem O6.160/O6.270; tie with O4.180 broken by higher expected Tc
  97.3 vs ~96)

## Budget reconciliation
search_db: 13 (Hg 9p + Ca1Cu2Ba2Hg1 4p). measure_tc (billable only): 20
  = Step A 10 (12 tried - 2 null free) + Step B 10 (14 tried - 4 null free).
Total = 33. (1 spare, unused.) All 26 unique materials characterized/predicted.

## Step A — seed homologous series (Ca1Cu2Ba2Hg1, varying O)
12 members found (search "Ca1Cu2Ba2Hg1", 4 pages):
1. O6.080Ca1Cu2Ba2Hg1  (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1
Element set {O,Ca,Cu,Ba,Hg} = 5 elements. Need to measure all → find max = T*.

## Step B/C/D candidate family: O4.xCu1Ba2Hg1 (4-element Hg: O,Cu,Ba,Hg)
Found 9 so far (Hg p1-3): O4.047, 0.066, 0.081, 0.085, 0.096, 0.097, 0.101, 0.103, 0.150.
Hg search = 27 total. 12 are seed series (5-elem) → 15 are O4.x (4-elem). Need 6 more.
These are the 4-element Hg materials for Step B. May also contain unmeasurable ones (Step C/D).

## Budget plan
- Searches: 7 done + 2 more (Hg p4-5 for remaining O4.x) = 9 total. (left 25)
- Step A: measure 12 → T*. (left 13)
- Step B/C/D: measure 13 of 15 O4.x (skip 2 extreme-doping, lowest-Tc, least likely Q1). (left 0)
Total = 34. Exactly. NO margin — must be careful.
T* is critical (drives threshold1) → protect it: measure all 12 Step A.
