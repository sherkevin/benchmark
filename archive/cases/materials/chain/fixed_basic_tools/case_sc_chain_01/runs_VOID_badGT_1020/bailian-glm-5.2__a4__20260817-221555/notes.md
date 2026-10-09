# Notes — Superconductor Tc study

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K, Low = Tc < 90 K.
- Budget: 34 measure_tc calls.
- measurable.txt: 29 compositions. unmeasurable.txt: 10 compositions.

## Key structural identifications (chemistry)
- `Ca1Cu2Ba2Hg1` series = **Hg-1212** (HgBa2Ca1Cu2, 2 CuO2 planes). Peak Tc ~127 K optimally doped.
- `Cu1Ba2Hg1` (O4.xxx) = **Hg-1201** (1 CuO2 plane). Tc up to ~94 K (pressure), ~40-73 ambient.
- `O10Ca2Cu3Ba2Tl2` = **Tl-2223** (3 planes), ~125 K.
- Y/La `Cu3Ba2` = Y-123 family. `Cu4` = Y-124 family (~80 K).

## CRITICAL overlap observation
4 of 10 "unmeasurable" compositions ALSO appear in measurable.txt:
1. O6.160Ca1Cu2Ba2Hg1 (series line 5)
2. O6.270Ca1Cu2Ba2Hg1 (series line 10)
3. O4.180Cu1Ba2Hg1 (measurable line 19)
4. O4.047Cu1Ba2Hg1 (measurable line 20)
=> These HAVE samples; I will measure them via the measurable list and use REAL Tc for step C (ground truth, no rule needed).

## Truly unmeasurable (6, need rule):
5. O4.066Cu1Ba2Hg1   (Hg-1201, low O)
6. O4.160Cu1Ba2Hg1   (Hg-1201, mid O)
7. O8Ca0.850Cu2.200Ba2Tl1.800   (Tl-2212-ish, 2 planes, O8)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500    (Tl/Pb-1212, Sr-based, 2 planes)
9. O8Cu3Y1Ba1.900La0.100        (Y-123-ish, 2 planes, O8)
10. O8Cu4Y1Ba1.870La0.130       (Y-124-ish, 2 planes, Cu4)

## Plan
- Step A: measure 12 series -> find T* (max).
- Step B: Hg + exactly 4 elements in measurable = 12 O4xxx. threshold1 = T*-32. Report those >= threshold1.
- Step C: measure 5 "other" materials to aid rule induction. Use all 29 measured as training set. Predict 6 truly unmeasurable; use real Tc for 4 overlap.
- Total calls: 12+12+5 = 29 <= 34. Buffer 5.

## Step A — series measurements (Hg-1212, O6.xxx)
| comp | Tc |
|---|---|
| O6.080 | 92.0 |
| O6.100 | 92.5 |
| O6.130 | 102.0 |
| O6.140 | 105.0 |
| O6.160 | **no sample** (overlap w/ unmeasurable) |
| O6.190 | 122.0 |
| O6.210 | **127.0** ← MAX (tie) |
| O6.220 | **127.0** ← MAX (tie) |
| O6.240 | 123.0 |
| O6.270 | **no sample** (overlap w/ unmeasurable) |
| O6.300 | 100.0 |
| O6.310 | 98.0 |

- T* = 127.0. Peak plateau at O6.210–O6.220.
- Interpolation: O6.160 (~115, between 105 & 122) and O6.270 (~110, between 123 & 100) — neither exceeds 127. T* = 127.0 confirmed.
- Budget counter shows no-sample calls do NOT consume budget (used=10 after 12 calls).
- Pick peak = O6.210Ca1Cu2Ba2Hg1 (first occurrence of max). T* = 127.0.

## Step B setup
- threshold1 = T* - 32 = 127 - 32 = **95.0 K**.
- Family B = contains Hg AND exactly 4 elements = 12× O4.xxx Cu1Ba2Hg1 (O,Cu,Ba,Hg).
- Measure all 12, report those with Tc >= 95.0.

## Step B — O4.xxx Cu1Ba2Hg1 (Hg-1201) measurements
threshold1 = 95.0 K. Bold = >= threshold1 (qualifies for Q1).
| comp | Tc | >=95? |
|---|---|---|
| O4.047 | **no sample** (overlap) | ? (predict from rule) |
| O4.081 | 98.0 | **YES** |
| O4.085 | 92.0 | no |
| O4.096 | 95.6 | **YES** |
| O4.097 | 95.3 | **YES** |
| O4.101 | 93.4 | no |
| O4.103 | 92.5 | no |
| O4.150 | 97.0 | **YES** |
| O4.162 | 97.4 | **YES** |
| O4.180 | **no sample** (overlap) | ? (predict from rule) |
| O4.200 | 94.2 | no |
| O4.220 | 97.0 | **YES** |

Measured Hg-1201 forms a dome: 92 (O4.085) → 98 (O4.081) → 95.6 (O4.096) → 97 (O4.150) → 97.4 (O4.162) → 97 (O4.220) → 94.2 (O4.200).
- The dome is flat ~95-98 across a wide O range. Extrapolation:
  - O4.180 (within measured range, ~95-97) → expect ~95-97 >= 95.0 → HIGH, qualifies.
  - O4.047 (below all measured, well below dome; matches seed behavior — the O6.080 extreme was 92) → expected low-mid. Use rule.

## Step C — measure "other" training materials
| comp | type | Tc | class |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | Hg-1201 + Tl (mixed heavy site) | 0.0 | LOW |
| O10Ca2Cu3Ba2Tl2 | Tl-2223 (clean Tl, 3 planes) | 118.97 | HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | Tl-1201 + Cd (Tl site sub) | 88.0 | LOW |
| O7Cu3Ba2La1 | La-123 (no Y, no Hg/Tl) | 89.6 | LOW |
| O7Cu3Y0.200Ba2La0.800 | YBCO, Y-poor (no Hg/Tl) | 81.0 | LOW |

Budget used = 25 / 34. (No-sample calls do not consume budget; only 25 of 29 attempted consumed budget.)

## Full training set (25 measured Tc)
HIGH (>=90): all 10 Hg-1212 (92-127), all 12 Hg-1201 (92-98), Tl-2223 (118.97). [23 high... wait 10+12+1=23? No: 10 Hg-1212 measured + 12 Hg-1201 measured + 1 Tl-2223 = 23 HIGH]
LOW (<90): Hg+Tl (0), Tl+Cd (88), La-123 (89.6), Y0.2La0.8 (81). [4 LOW]
Total measured-with-Tc = 27? No: 10+12+5 = 27 attempted-with-Tc... recount: 12 Hg-1212 attempted, 10 have Tc (2 no-sample); 12 Hg-1201 attempted, 10 have Tc (2 no-sample); 5 others all have Tc. => 10+10+5 = 25 with Tc. ✓ matches budget 25.

## Rule induction (Step C)
Single factors that FAIL:
- "contains Hg": wrong — Hg+Tl mix (O6Cu1Ba2Hg1Tl1) = 0 (LOW) despite Hg.
- "contains Tl": wrong — Tl+Cd (88) LOW, Hg+Tl (0) LOW despite Tl.
- "has 5 elements": wrong — Hg-1212 (5, HIGH) and Hg+Tl (5, LOW); Tl-2223 (5, HIGH) and YBCO-La (5, LOW).
- "#CuO2 planes >= 2": wrong — Hg-1201 (1 plane, HIGH) vs Tl+Cd (1 plane, LOW).

INDUCED RULE (multi-factor): **HIGH Tc (>=90 K) requires a clean mercury/thallium cuprate: a single, unmixed Hg (or Tl) as the heavy p-block cation in the charge-reservoir layer, together with Ba (NOT Sr) and CuO2 planes. Materials are LOW if they (a) have NO Hg/Tl (Y/La rare-earth cuprates), (b) MIX a second cation at the heavy site (Hg+Tl, Tl+Cd, Tl+Pb), or (c) use Sr instead of Ba.** Equivalently: clean Hg or clean Tl + Ba + CuO2 => HIGH; everything else LOW.

Validation against training:
- Hg-1212 (clean Hg+Ba+Ca) => HIGH ✓; Hg-1201 (clean Hg+Ba) => HIGH ✓; Tl-2223 (clean Tl+Ba+Ca) => HIGH ✓.
- Hg+Tl (mixed heavy) => LOW ✓; Tl+Cd (Cd at Tl site) => LOW ✓; La-123 & YBCO-La (no Hg/Tl) => LOW ✓.

## Predictions for the 10 unmeasurable
Apply the rule. Note: 4 "unmeasurable" also appear in measurable.txt but returned no-sample (never synthesized) — predict via rule (consistent with dome interpolation for the Hg series).
1. O6.160Ca1Cu2Ba2Hg1 — clean Hg+Ba (Hg-1212, mid-dome ~115) => **high**
2. O6.270Ca1Cu2Ba2Hg1 — clean Hg+Ba (Hg-1212, ~110) => **high**
3. O4.180Cu1Ba2Hg1 — clean Hg+Ba (Hg-1201, dome ~96) => **high**
4. O4.047Cu1Ba2Hg1 — clean Hg+Ba (Hg-1201, low-O edge ~90) => **high**
5. O4.066Cu1Ba2Hg1 — clean Hg+Ba (Hg-1201, ~92) => **high**
6. O4.160Cu1Ba2Hg1 — clean Hg+Ba (Hg-1201, dome peak ~97) => **high**
7. O8Ca0.850Cu2.200Ba2Tl1.800 — clean Tl (Tl1.8 vacancies, not site-mixed) + Ba + Ca, 2 planes (Tl-2212 ~112) => **high**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl/Pb 50/50 MIXING at heavy site + **Sr (not Ba)** => **low**
9. O8Cu3Y1Ba1.900La0.100 — no Hg/Tl (Y/La Y-123 family); O8 overdoped + La-on-Ba suppresses => **low**
10. O8Cu4Y1Ba1.870La0.130 — no Hg/Tl (Y-124 family, ~80 K) => **low**

HIGH predictions: #1-7 (six Hg-based + Tl-2212). LOW: #8-10.

## Step D — final pick
Among predicted HIGH, fewest elements:
- O4.047, O4.066, O4.160, O4.180 (Hg-1201) = 4 elements each (fewest)
- O6.160, O6.270 = 5; O8Ca...Tl = 5
Tie-break (4-way) by higher expected Tc:
- O4.160 ~97.2 (at dome peak, neighbors O4.150=97.0, O4.162=97.4) ← HIGHEST
- O4.180 ~96 (between O4.162=97.4 and O4.200=94.2)
- O4.066 ~92-95 (low-O edge)
- O4.047 ~90 (lowest, low-O edge)
=> **final = O4.160Cu1Ba2Hg1**

## Summary
- peak = O6.210Ca1Cu2Ba2Hg1 (ties with O6.220 at 127.0; reporting first)
- T* = 127.0
- threshold1 = 95.0
- Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1 (measured Hg-1201 with Tc>=95.0)
- predictions: 10 entries (7 high, 3 low)
- final = O4.160Cu1Ba2Hg1
