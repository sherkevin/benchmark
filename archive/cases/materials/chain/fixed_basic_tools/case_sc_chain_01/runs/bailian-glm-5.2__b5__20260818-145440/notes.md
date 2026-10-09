# Superconductor Tc study — notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- **high** = Tc >= 90 K, else **low**
- Budget: 18 measure_tc calls
- Step A: find MAX Tc in homologous doping series (same elements {O,Ca,Cu,Ba,Hg}, varying O) = the 1212 series (lines 1-10 of measurable.txt). Call max T*.
- Step B: family B = contains Hg AND exactly 4 elements = the 1201 series (Cu1Ba2Hg1, lines 11-21, 11 members). threshold1 = T* - 32. Q1 = all family-B members with Tc >= threshold1.
- Step C: induce a rule (multi-factor interaction, NOT single-factor) to predict high/low for 7 unmeasurable materials.
- Step D: among predicted-high unmeasurables, pick fewest elements (tie-break higher expected Tc).

## Measurable materials
1212 series (Hg, 5 elem): O6.080/6.100/6.130/6.140/6.190/6.210/6.220/6.240/6.300/6.310 Ca1Cu2Ba2Hg1
1201 series (Hg, 4 elem): O4.047/4.085/4.096/4.097/4.101/4.103/4.150/4.162/4.180... wait 4.180 is unmeasurable. Measurable 1201: O4.047/4.085/4.096/4.097/4.101/4.103/4.150/4.162/4.200/4.220 Cu1Ba2Hg1 (lines 11-21 = 11 members)
References (non-Hg or multi): 
- L22 O6Cu1Ba2Hg1Tl1 (Hg+Tl, 5elem, Cu1)
- L23 O10Ca2Cu3Ba2Tl2 (Tl2, Cu3)
- L24 O6Cu1Cd0.250Ba2Tl1.750 (Tl+Cd, Cu1)
- L25 O7Cu3Ba2La1 (La, Cu3)
- L26 O7Cu3Y0.200Ba2La0.800 (Y+La, Cu3)

## Unmeasurable (7)
1. O6.160Ca1Cu2Ba2Hg1  (1212, Cu2) — interpolate in 1212 series
2. O6.270Ca1Cu2Ba2Hg1  (1212, Cu2) — interpolate in 1212 series
3. O4.180Cu1Ba2Hg1     (1201, Cu1) — interpolate in 1201 series
4. O8Ca0.850Cu2.200Ba2Tl1.800 (Tl, Cu2.2, Ca)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (Tl+Pb, Sr2, Cu2)
6. O8Cu3Y1Ba1.900La0.100 (Y+La, Cu3)
7. O8Cu4Y1Ba1.870La0.130 (Y+La, Cu4)

## Budget plan (18 calls) — FINAL
1212 (Step A peak + Step C #1,#2 interp): measured 5 spread points → 92,105,122,123,98. Peak=123 at O6.240. (T*=123; 6.220 unmeasured but interpolated ~123.)
1201 (Step B Q1, all 11 + Step C #3 interp): measure all 11 (calls 6-16).
References (Step C rule for #4-7): pick best 2 of remaining calls.
- Key tension: T* accuracy (6.220) vs reference data (L23 Tl, L26 Y).
- Decision: measure 1201 first; if values near 91-93 boundary, need 6.220; else spend on 2 refs.

## 1212 measured (Step A)
| comp | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 ← max (T*=123) |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |
Unmeasured 1212: 6.100,6.130,6.210,6.220,6.300 — by interpolation all ≤ ~123, peak region ~6.22-6.24.
threshold1 = 123 - 32 = 91.0

## 1201 series measured (Step B), sorted by O
| comp | Tc | >=91.0? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | Y |
| O4.081Cu1Ba2Hg1 | 98.0 | Y |
| O4.085Cu1Ba2Hg1 | 92.0 | Y |
| O4.096Cu1Ba2Hg1 | 95.6 | Y |
| O4.097Cu1Ba2Hg1 | 95.3 | Y |
| O4.101Cu1Ba2Hg1 | 93.4 | Y |
| O4.103Cu1Ba2Hg1 | 92.5 | Y |
| O4.150Cu1Ba2Hg1 | 97.0 | Y |
| O4.162Cu1Ba2Hg1 | 97.4 | Y |
| O4.200Cu1Ba2Hg1 | 94.2 | Y |
| O4.220Cu1Ba2Hg1 | 97.0 | Y |
Min 1201 Tc = 92.0 ≥ 91.0 → **ALL 11 qualify = Q1** (entire 1201 series).

## Budget status: 16 used, 2 left.
Remaining for Step C rule: need data on the interaction (Hg-basis family ~98; Cu2/1212 ~123; references needed).
Best reference picks to disambiguate the rule:
 - O6Cu1Ba2Hg1Tl1 (has Hg+Cu1, mixed 1201-like; should be HIGH ~95-98)
 - O10Ca2Cu3Ba2Tl2 (Tl2+Cu3, no Hg; canonical 1223 Tl ~120 → HIGH)
 - O7Cu3Ba2La1 (Y-free La analog of L26; Cu3+La → high ~92)
Tl2+Cu3 (L23) is the best discriminator for #4 (Tl1.8, Cu2.2, no Hg).
La analog (L25) is best for #6/#7 (Cu3+4, Y+La, no Hg).
Plan: measure L23 (O10Ca2Cu3Ba2Tl2) and L25 (O7Cu3Ba2La1).

## Reference measurements (Step C rule induction) — budget 17,18
- O10Ca2Cu3Ba2Tl2 = 118.97 → HIGH. (Pure Tl2 double-layer, Tl2-1223 family, Cu3, Ca2, Ba2, O10.)
- O6Cu1Ba2Hg1Tl1 = 0.0 → LOW. (MIXES Hg+Tl in rock-salt; Cu1 with O6 and NO Ca → structurally inconsistent: O6 implies n=2 1212-type but Cu1/none-Ca imply n=1 1201-type.)

## Induced RULE (multi-factor interaction, NOT single-factor)
HIGH iff the material is a structurally COMPLETE member of a high-Tc cuprate homologous family with a SINGLE primary rock-salt heavy cation — i.e. pure Hg (Hg-12nn-1, n≥1), or the Tl2 double-layer family (Tl2-12nn-1, n≥2), or the Y-Ba-123 (Cu3) structure — at an internally consistent oxygen / alkaline-earth (Ca/Sr) stoichiometry.
LOW when any of: (a) two different heavy cations share the rock-salt (Hg+Tl → measured 0; analogously Tl+Pb), (b) Cu-layer count n is inconsistent with O/Ca (defective structure, e.g. Cu1+O6+no-Ca), or (c) a known low-Tc variant — specifically the Y-Ba-124 (Cu4) structure (~80 K < 90).
Why not single-factor: "contains Hg" wrongly calls L22 (0 K) high and wrongly calls #4 Tl2-1212 & #6 YBCO-123 low; "has 5 elements" wrongly calls #7 (Y-124, 5 elem, ~80 K) high. The real separator is cation-type × structure-completeness × Cu-count.

## Predictions for the 7 unmeasurable
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (Hg1212, pure Hg, valid n=2; interps 6.140=105 & 6.190=122 → ~115)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (Hg1212, pure Hg; interps 6.240=123 & 6.310=98 → ~113)
3. O4.180Cu1Ba2Hg1 → HIGH (Hg1201, pure Hg; interps 4.162=97.4 & 4.200=94.2 → ~96)
4. O8Ca0.850Cu2.200Ba2Tl1.800 → HIGH (Tl2-1212: Tl~2 single cation, Cu2, Ca, Ba2, O8 — ~108 K)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → LOW (mixes two heavy cations Tl+Pb in rock-salt, like L22's Hg+Tl=0)
6. O8Cu3Y1Ba1.900La0.100 → HIGH (Y-Ba-123 / YBCO structure, Cu3, ~92 K; La substitutes Ba site)
7. O8Cu4Y1Ba1.870La0.130 → LOW (Y-Ba-124 structure, Cu4, ~80 K < 90)

## Step D — final pick
Predicted-HIGH set = {#1,#2,#3,#4,#6}. Element counts:
- #1 = 5, #2 = 5, #3 = 4 (O,Cu,Ba,Hg), #4 = 5, #6 = 5.
FEWEST elements = #3 (4 elements, uniquely). No tie-break needed.
**FINAL = O4.180Cu1Ba2Hg1**
(Robust: #5(6 elem) and #7(5 elem) classifications don't affect Step D — #3 still has fewest elements among any plausible HIGH set.)

## Step A / B final
T* = 123.0 (measured max; O6.240Ca1Cu2Ba2Hg1). True peak likely ~123-124 near O6.220, but only measured values may be cited → T*=123.0, peak=O6.240Ca1Cu2Ba2Hg1.
threshold1 = 123 - 32 = 91.0.
Q1 (all family-B members with Tc ≥ 91.0) = ALL 11 of the 1201 series (min 92.0 ≥ 91.0):
O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220 (each Cu1Ba2Hg1).
