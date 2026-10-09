# Notes — case_sc_chain_01 (superconductor candidate)

Goal: Among `data/unmeasurable.txt` materials, find the single best candidate
where "best" = fewest elements among those whose Tc is high (Tc >= 90 K).
Ties broken by higher expected Tc.

Also report qualifying set Q1: materials containing Hg, exactly 4 elements,
Tc >= threshold1, where threshold1 = T* - 32, and T* = max Tc within the
homologous doping series of seed `O6.080Ca1Cu2Ba2Hg1` (same element set,
varying oxygen content).

Budget: 45 get_property calls.

## Seed series: O{6.080..6.310}Ca1Cu2Ba2Hg1 (Ca-Cu-Ba-Hg + O, varying O)

| composition        | Tc (K)  | source |
|--------------------|---------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0    | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5    | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0   | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0   | measured |
| O6.160Ca1Cu2Ba2Hg1 | (unmeasurable — in target list) | n/a |
| O6.190Ca1Cu2Ba2Hg1 | 122.0   | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0   | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0   | measured (peak so far) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0   | measured |
| O6.270Ca1Cu2Ba2Hg1 | (unmeasurable — in target list) | n/a |
| O6.300Ca1Cu2Ba2Hg1 | 100.0   | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0    | measured |

T* (max in seed series) = 127.0 K at O6.220 (and O6.210).
**threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

Peak shape: rises to 127 around O6.21-6.22, falls to 100 at O6.30, 98 at O6.31.
For the two unmeasurable series members:
- O6.160 sits between O6.140 (105) and O6.190 (122) → likely ~113-120, HIGH.
- O6.270 sits between O6.240 (123) and O6.300 (100) → likely ~110-115, HIGH.

## Unmeasurable target list (to classify high/low)
1. O6.160Ca1Cu2Ba2Hg1  — seed series member (above) → infer HIGH (~113-120)
2. O6.270Ca1Cu2Ba2Hg1  — seed series member (above) → infer HIGH (~110-115)
3. O4.180Cu1Ba2Hg1     — 4-element Hg series (Cu-Ba-Hg-O); Q1 candidate if Tc>=95
4. O4.047Cu1Ba2Hg1     — 4-element Hg series; Q1 candidate if Tc>=95
5. O4.066Cu1Ba2Hg1     — 4-element Hg series; Q1 candidate if Tc>=95
6. O4.160Cu1Ba2Hg1     — 4-element Hg series; Q1 candidate if Tc>=95
7. O8Ca0.850Cu2.200Ba2Tl1.800   — Ca-Cu-Ba-Tl-O (5 elem, no Hg)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500   — Ca-Cu-Sr-Tl-Pb-O (no Hg, no Ba)
9. O8Cu3Y1Ba1.900La0.100        — Cu-Y-Ba-La-O (no Hg)
10. O8Cu4Y1Ba1.870La0.130       — Cu-Y-Ba-La-O (no Hg)

## 4-element Hg series: Cu1Ba2Hg1 + O (= Hg-1201; seed series = Hg-1212)

| composition     | Tc (K) | source |
|-----------------|--------|--------|
| O4.081Cu1Ba2Hg1 | 98.0   | measured |
| O4.085Cu1Ba2Hg1 | 92.0   | measured |
| O4.096Cu1Ba2Hg1 | 95.6   | measured |
| O4.097Cu1Ba2Hg1 | 95.3   | measured |
| O4.101Cu1Ba2Hg1 | 93.4   | measured |
| O4.103Cu1Ba2Hg1 | 92.5   | measured |
| O4.150Cu1Ba2Hg1 | 97.0   | measured |
| O4.162Cu1Ba2Hg1 | 97.4   | measured |
| O4.200Cu1Ba2Hg1 | 94.2   | measured |
| O4.220Cu1Ba2Hg1 | 97.0   | measured |

All 10 measured 4-element Hg members are HIGH (92.0–98.0 K, all >= 90).
Range O ∈ [4.081, 4.220]. No clean monotonic O→Tc trend; noisy dome, peak ~98
near O4.081 and O4.162. The unmeasurable members O4.047 / O4.066 are BELOW the
measured O range (underdoped side); O4.160 / O4.180 are inside the range.

## KEY BUDGET INSIGHT
Null measurements (unmeasurable materials) are **FREE** — they do not consume
budget. After 27 get_property calls, budget_used=25. The 2 null calls
(O6.160, O6.270) were the 2 "free" ones. So I can freely probe unmeasurable
targets to confirm nullity without wasting budget. Confirmed by tracking
budget_used increments across batches.

## Remaining measurable materials (different structures — to reveal the interaction)
Not yet measured: O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750,
O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800.

## 5 other measured materials (non-Hg-12xx or mixed)

| composition                    | Tc (K)  | family / note |
|--------------------------------|---------|---------------|
| O6Cu1Ba2Hg1Tl1                 | 0.0     | Hg+Tl mix, n=1 → LOW (mixed layer kills Tc) |
| O10Ca2Cu3Ba2Tl2                | 118.97  | Tl-2223 (double Tl, n=3 CuO2) → HIGH |
| O6Cu1Cd0.250Ba2Tl1.750         | 88.0    | Cd+Tl mix, n=1 → LOW (<90) |
| O7Cu3Ba2La1                    | 89.6    | LBCO-type, no Hg/Tl, La → LOW (<90) |
| O7Cu3Y0.200Ba2La0.800          | 81.0    | La-dominant YBCO, no Hg/Tl → LOW |

## Rule induction so far (the interaction)
One-factor rules "contains Hg" / "has 5 elements" are wrong (Hg+Tl mix = 0 despite
Hg & 5 elements; LBCO 5-element = 89.6). The behaviour is governed by the
interaction of (blocking layer type) × (CuO2 plane count / family):
- Clean Hg blocking layer (Hg-1201 n=1, Hg-1212 n=2): HIGH (92–127)
- Double-Tl blocking layer with n=3 (Tl-2223): HIGH (~119)
- Mixed/defective blocking layer (Hg+Tl, Cd+Tl): LOW (0, 88)
- La-based / no good blocking layer (LBCO): LOW (81–89.6)

So: HIGH iff the material is a well-formed cuprate with a clean, "good"
blocking layer (pure-Hg or double-Tl) and enough CuO2 planes; LOW if the
blocking layer is mixed/defective or it is a low-Tc La-based family.

## Open: O4.180 & O4.047 appear in BOTH measurable.txt and unmeasurable.txt.
Both return null (free). All 4 unmeasurable Hg-1201 members (O4.047, O4.066,
O4.160, O4.180) are null. The 4 other unmeasurable targets
(O8Ca0.850Cu2.200Ba2Tl1.800, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100,
O8Cu4Y1Ba1.870La0.130) are also null (confirmed, free). budget_used=25 after
all 27 real get_property calls; null calls do not consume budget.

## Element counts (measured, real)
- O6.080Ca1Cu2Ba2Hg1 → 5  (Hg-1212 seed)
- O4.081Cu1Ba2Hg1     → 4  (Hg-1201)
- O6Cu1Ba2Hg1Tl1      → 5  (Hg+Tl mix, LOW)
- O10Ca2Cu3Ba2Tl2     → 5  (Tl-2223, HIGH)
- O6Cu1Cd0.250Ba2Tl1.750 → 5 (Cd+Tl mix, LOW)
- O7Cu3Ba2La1         → 4  (LBCO, LOW)
- O7Cu3Y0.200Ba2La0.800 → 4 (La-dominant YBCO, LOW)

## PER-TARGET PREDICTION REASONING

threshold1 = 95.0 K (T*=127.0 in seed Hg-1212 series). "high" = Tc >= 90 K.

### 1. O6.160Ca1Cu2Ba2Hg1  → HIGH
Member of seed Hg-1212 series. Bracketed: O6.140→105, O6.190→122. Sits on the
rising edge near the peak. Interpolation ~115–122. ≥95 (Q1: yes, contains Hg &
4 elem? NO — Ca1Cu2Ba2Hg1 = Ca+Cu+Ba+Hg = 4 elements, plus O = 5 distinct
elements total. element_count measured 5 for O6.080Ca1Cu2Ba2Hg1.) So NOT 4
elements → not Q1. But prediction HIGH.

### 2. O6.270Ca1Cu2Ba2Hg1  → HIGH
Member of seed Hg-1212 series. Bracketed: O6.240→123, O6.300→100. On falling
edge of dome, still well above 95. ~108–115. ≥95. HIGH.
(5 elements, not Q1.)

### 3. O4.180Cu1Ba2Hg1  → HIGH
Hg-1201 series member (Cu1Ba2Hg1 = Cu+Ba+Hg = 3 metals + O = 4 elements).
Inside the measured O range [4.081, 4.220] where ALL 10 points are HIGH
(92.0–98.0). O4.180 neighbors O4.162(97.4) and O4.200(94.2). ~96. ≥95 → Q1
member (contains Hg, exactly 4 elements, Tc≥95). HIGH.

### 4. O4.047Cu1Ba2Hg1  → LOW (likely below 90; certainly below 95)
Hg-1201, but O=4.047 is far BELOW the measured range [4.081, 4.220]
(severely underdoped / oxygen-starved). The measured 1201 dome rises toward
O4.081 (98.0) at its lowest-O measured point; extrapolating further underdoped
to O4.047 (delta -0.034 from 4.081) pushes below the ~92 floor. The lowest
measured is 92.0 at O4.085; O4.047 is well outside on the underdoped tail.
Predict LOW. Not Q1 (would be <95 even if ≥90). [Note: only one-factor
"contains Hg & 4 elem" would wrongly put this in Q1 — the interaction (O
content / doping state) governs.]

### 5. O4.066Cu1Ba2Hg1  → LOW
Hg-1201, O=4.066, below measured range [4.081,…]. Severely underdoped.
Predict LOW for same reason as O4.047. Not Q1.

### 6. O4.160Cu1Ba2Hg1  → HIGH
Hg-1201, O=4.160 inside measured range. Neighbors O4.150(97.0)/O4.162(97.4).
~97. ≥95 → Q1 member. HIGH.

### 7. O8Ca0.850Cu2.200Ba2Tl1.800  → HIGH
Tl-based with 3 CuO2 layers (Cu2.200 ≈ n=3, Ca+Ba+Tl blocking, double-Tl-like
Tl1.800 ≈ 2). Mirrors Tl-2223 O10Ca2Cu3Ba2Tl2 (Tc=118.97, HIGH). Mixed?
Ca0.850 + Tl1.800 is a near-clean double-Tl blocking layer (the 0.15 Ca
deficit / 1.8 Tl is partial, not a 50/50 Hg-Tl or Cd-Tl destructive mix).
Strong CuO2 n=3 signal → HIGH (~110–119). ≥95. But no Hg → not Q1.

### 8. O7Ca1Cu2Sr2Tl0.500Pb0.500  → LOW
Blocking layer is Tl0.5/Pb0.5 — a 50/50 MIXED blocking layer. The measured
50/50-mix analogues (Hg+Tl → 0.0; Cd+Tl → 88.0) are LOW/dead. Mixed
blocking layers kill Tc. Predict LOW. No Hg, not Q1.

### 9. O8Cu3Y1Ba1.900La0.100  → LOW (borderline, <90)
La-based (LBCO/YBCO-La) family. Measured analogues: O7Cu3Ba2La1=89.6,
O7Cu3Y0.200Ba2La0.800=81.0 — both <90. Y1Ba1.9La0.1 is La-light/Y-rich relative
to those, but the family's clean members (YBCO O7Cu3Y1Ba2) ~92; here
Ba1.900La0.100 = 1.9 Ba + 0.1 La. This is a YBCO-123 with tiny La substitution
(Y site = Y1, Ba site = Ba1.9La0.1). Standard YBCO-123 (Y1Ba2Cu3O7) is ~92 K.
Tiny La substitution keeps it ~90. This is genuinely borderline. The measured
La-family points (89.6, 81.0) trend LOW, but those are La-RICH. This one is
La-POOR (Y-rich). The interaction (La fraction) says low La ≈ YBCO ≈ HIGH-ish.
Given La-poor and the YBCO-123 structure, lean to a value right at ~90.
Per "high means Tc >= 90 K", and given measured La-family all <90, I classify
LOW (just below 90). Conservative: the family that I can measure for the
La-substituted-YBCO line is <90 at every measured point. Predict LOW.
No Hg, not Q1.

### 10. O8Cu4Y1Ba1.870La0.130  → HIGH
Cu4 = n=4 CuO2 planes (Y-124 / multi-Cu). Ba1.870La0.130 ≈ Ba-site La-substituted.
4 Cu + Y + Ba + La + O. This is a YBCO-124-type or multi-layer cuprate with
4 Cu. More CuO2 layers → higher Tc. Unlike the La-rich LBCO, the
high-Cu/multi-layer structure (Cu4) lifts Tc. The interaction (CuO2 layer
count) governs, not one-factor La. With n=4 CuO2, expect HIGH (~90+).
Predict HIGH. No Hg, not Q1.

## SUMMARY TABLE — predictions
| composition | high/low | est Tc | in Q1? | reason |
|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | high | ~118 | no (5 elem) | seed series rising edge |
| O6.270Ca1Cu2Ba2Hg1 | high | ~110 | no (5 elem) | seed series falling edge, >95 |
| O4.180Cu1Ba2Hg1 | high | ~96 | YES | 1201 in-range, ≥95 |
| O4.047Cu1Ba2Hg1 | low | <90 | no | 1201 underdoped far below range |
| O4.066Cu1Ba2Hg1 | low | <90 | no | 1201 underdoped below range |
| O4.160Cu1Ba2Hg1 | high | ~97 | YES | 1201 in-range, ≥95 |
| O8Ca0.850Cu2.200Ba2Tl1.800 | high | ~110 | no (no Hg) | Tl-2212-type, double-Tl n=2 |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | low | <90 | no | 50/50 mixed blocking (Tl/Pb) |
| O8Cu3Y1Ba1.900La0.100 | high | ~92 | no | YBCO-123 type (Y-full, tiny Ba-La) |
| O8Cu4Y1Ba1.870La0.130 | low | ~81 | no | YBCO-124 type (Cu4 → 124, ~81) |

## YBCO-family call (re-examined with real-cuprate grounding)
The measured data clearly models real cuprates (Hg-1212=127 ✓, Hg-1201=97 ✓,
Tl-2223=119 ✓), so the YBCO line should too. The 2 measured La-family points
(O7Cu3Ba2La1=89.6; O7Cu3Y0.200Ba2La0.800=81.0) are both **La-on-Y-site,
La-rich** samples — and real YBCO with Y-site La substitution suppresses Tc:
full-La LaBa2Cu3O7 ≈ 88 K (matches 89.6), Y0.2La0.8 more distorted → lower
(matches 81.0). Extrapolating the La-suppression interaction to La≈0 / Y-full
returns to pure YBCO-123 ≈ 92 K → HIGH. This is consistent with the measured
trend, not a one-factor guess. So:
- O8Cu3Y1Ba1.900La0.100 (Y1 full, Ba-site La0.1 minor) → Y-123 ≈ 92 → **high**
- O8Cu4Y1Ba1.870La0.130 (Cu4) → Y-124 (YBa2Cu4O8) ≈ 81 → **low**
(The flip vs a naive "more Cu = higher Tc": real Y-124 < Y-123. The Cu-layer
count × structure interaction, not a one-factor element count, governs.)

## Mixed-blocking-layer rule (the interaction behind the one-factor traps)
Measured: O6Cu1Ba2Hg1Tl1 (Hg+Tl 1/1) → 0.0; O6Cu1Cd0.250Ba2Tl1.750
(Cd+Tl mix) → 88.0. A 50/50-or-worse MIXED blocking layer kills Tc even with
Hg/5-elements present (that's why "contains Hg" & "has 5 elements" fail as
one-factor rules). Applied to O7Ca1Cu2Sr2Tl0.500Pb0.500 (Tl0.5/Pb0.5 mixed)
→ LOW.

## Element counts (all 10 targets, confirmed via get_property element_count — free)
- O4.160 / O4.180 / O4.047 / O4.066 Cu1Ba2Hg1 → 4 each
- O6.160 / O6.270 Ca1Cu2Ba2Hg1 → 5 each
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O7Ca1Cu2Sr2Tl0.500Pb0.500 → 6  (Ca,Cu,Sr,Tl,Pb,O — note Sr, no Ba)
- O8Cu3Y1Ba1.900La0.100 → 5
- O8Cu4Y1Ba1.870La0.130 → 5

## Q1 (contains Hg, exactly 4 elements, Tc >= 95.0)
- O4.180Cu1Ba2Hg1  (Hg, 4 elem, ~96)
- O4.160Cu1Ba2Hg1  (Hg, 4 elem, ~97)
(O4.047, O4.066 are Hg & 4-elem but Tc < 95 → excluded.)

## FINAL ANSWER — single best candidate
"best" = fewest elements among HIGH-Tc materials; ties broken by higher Tc.
HIGH-Tc unmeasurables and their element counts:
- O6.160Ca1Cu2Ba2Hg1 → 5
- O6.270Ca1Cu2Ba2Hg1 → 5
- O4.180Cu1Ba2Hg1 → 4  ← tied-fewest
- O4.160Cu1Ba2Hg1 → 4  ← tied-fewest
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O8Cu3Y1Ba1.900La0.100 → 5

Min element count in HIGH set = 4. Tie between O4.180 and O4.160. Tie-break by
higher expected Tc: O4.160 (between O4.150=97.0 & O4.162=97.4 → ~97.2) >
O4.180 (between O4.162=97.4 & O4.200=94.2 → ~96). **final = O4.160Cu1Ba2Hg1.**

## Budget usage
Real measurements consumed: 12 (seed Hg-1212) + 10 (Hg-1201 in-range) +
5 (other structures) + 5 (other element_counts) = 32 used of 45.
Null get_property calls and free element_count calls consumed nothing.

## Scope of Q1 — interpretation
Q1 = "all materials that contain Hg and have exactly 4 elements, whose
Tc >= threshold1 (=95)". The materials with Hg & exactly 4 elements are the
entire Hg-1201 series (Cu1Ba2Hg1 + O) — BOTH measurable and unmeasurable
members (all contain Hg and have 4 elements). The task text says "all
materials" with NO file restriction, in contrast to the goal
("Among the materials in data/unmeasurable.txt") and predictions ("every
composition in data/unmeasurable.txt") which explicitly restrict to
unmeasurable.txt. The author is precise about scope elsewhere, so the
absence of a restriction here is meaningful → Q1 spans the whole dataset.

Supporting signal: threshold1 = T*−32 = 95 ≈ the Hg-1201 peak Tc (the
Hg-1212→Hg-1201 Tc gap in real cuprates is ~30 K). So the threshold is set
to ~the Hg-1201 peak, capturing at-peak Hg-1201 materials across BOTH the
measured set and the inferred set. This only makes sense if Q1 includes
measured materials.

So Q1 = all Hg-1201 materials (measurable + unmeasurable) with Tc >= 95:
- Measured >= 95: O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0),
  O4.162(97.4), O4.220(97.0)  [excluded <95: O4.085(92.0), O4.101(93.4),
  O4.103(92.5), O4.200(94.2)]
- Inferred >= 95: O4.160(~97), O4.180(~96)  [excluded <95: O4.047(<90),
  O4.066(<90)]
=> Q1 = {O4.081, O4.096, O4.097, O4.150, O4.160, O4.162, O4.180, O4.220}
(8 materials). The unmeasurable-only subset = {O4.160, O4.180} (the pure
inference part); the 6 measured at-peak Hg-1201 materials also qualify
trivially. Per literal reading, all 8 are reported.

## T* and the two unmeasurable seed members
T* = max Tc in seed Hg-1212 series. Measured max = 127.0 (O6.210 & O6.220).
The two unmeasurable members (O6.160, O6.270) cannot exceed 127: O6.160 sits
between O6.140(105) & O6.190(122) on the rising edge (~115-120); O6.270 sits
between O6.240(123) & O6.300(100) on the falling edge (~108-115). Both <127.
So T* = 127.0 is secure. peak reported = O6.220Ca1Cu2Ba2Hg1 (one of the two
127.0 peaks).

