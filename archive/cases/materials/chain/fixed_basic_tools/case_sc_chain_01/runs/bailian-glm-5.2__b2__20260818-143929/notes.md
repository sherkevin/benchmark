# Superconductor Tc study — running notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K; Low = Tc < 90 K
- Budget: 18 `measure_tc` calls

## Data inventory
### measurable.txt
- O6 series (Hg-1212 type; elements {O,Ca,Cu,Ba,Hg}, only O varies) — 10 members (lines 1–10):
  - O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310 (all Ca1Cu2Ba2Hg1)
- O4 series (Hg-1201 type; elements {O,Cu,Ba,Hg} = exactly 4 elements, contains Hg) — 11 members (lines 11–21):
  - O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220 (all Cu1Ba2Hg1)
- Other families (lines 22–26):
  - 22 O6Cu1Ba2Hg1Tl1        (5 el: O,Cu,Ba,Hg,Tl) — n=1 Hg/Tl-1201
  - 23 O10Ca2Cu3Ba2Tl2        (5 el: O,Ca,Cu,Ba,Tl) — Tl-2223 (n=3)
  - 24 O6Cu1Cd0.250Ba2Tl1.750 (5 el: O,Cu,Cd,Ba,Tl) — Tl/Cd-1201 (n=1)
  - 25 O7Cu3Ba2La1            (4 el: O,Cu,Ba,La) — La-123 (n=3), NO Hg
  - 26 O7Cu3Y0.200Ba2La0.800  (5 el: O,Cu,Y,Ba,La) — (Y,La)-123 (n=3)

### unmeasurable.txt (7; no sample)
1. O6.160Ca1Cu2Ba2Hg1  — interpolates in O6 series (between 0.140 & 0.190)
2. O6.270Ca1Cu2Ba2Hg1  — interpolates in O6 series (between 0.240 & 0.300)
3. O4.180Cu1Ba2Hg1     — interpolates in O4 series (between 0.162 & 0.200)
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-1212 (n=2, Cu2) variant
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — Tl/Pb/Sr-1212 (n=2, Cu2) variant
6. O8Cu3Y1Ba1.900La0.100      — Y-123 (n=3, Cu3) Ba/La on Ba site
7. O8Cu4Y1Ba1.870La0.130      — Y-124 (n=4, Cu4) extra Cu-O layer

## Step mapping
- A: measure O6 series (10) → T* = max Tc.
- B: threshold1 = T*−32. Q1 = {measurable materials with Hg AND exactly 4 elements AND Tc>=threshold1} ⇒ candidates = O4 series (11). Need measured Tc for those that qualify.
- C: induce rule (multi-factor interaction) separating high/low from measured set; predict all 7 unmeasurable.
- D: among predicted-high unmeasurable, pick fewest elements (tie → higher expected Tc).

## Budget plan (18 total)
- 10 → O6 series (Step A + interpolate #1,#2)
- remaining 8 → split between O4 (Step B Q1 + interpolate #3) and cross-family (lines 22–26) for the rule.
- Tension: O4 has 11 members, cross-family 5. Need rule to predict #4–#7.

## Step A results — O6 series (Hg-1212, n=2) [10 calls, budget 10/18]
| O content | Tc (K) |
|---|---|
| 6.080 | 92.0 |
| 6.100 | 92.5 |
| 6.130 | 102.0 |
| 6.140 | 105.0 |
| 6.190 | 122.0 |
| 6.210 | 127.0 |
| 6.220 | 127.0  ← peak |
| 6.240 | 123.0 |
| 6.300 | 100.0 |
| 6.310 | 98.0 |

**T\* = 127.0 K** (peak composition `O6.220Ca1Cu2Ba2Hg1`; O6.210 ties at 127.0).
**threshold1 = T\* − 32 = 95.0 K.**
All 10 O6 members are HIGH (>=90): min 92.0 at O6.080. Dome shape: rises 92→127 (0.080→0.220), falls 127→98 (0.220→0.310).

## Step C interpolation for O6-series unmeasurable
- #1 O6.160: between O6.140(105) & O6.190(122) → ~105 + (0.020/0.050)·17 ≈ 111.8 → **HIGH**
- #2 O6.270: between O6.240(123) & O6.300(100) → ~123 + (0.030/0.060)·(−23) ≈ 111.5 → **HIGH**

## Step B — Q1 candidates (Hg ∧ exactly 4 elements)
= O4 series (Hg-1201, n=1): 11 members. Need Tc >= 95.0.
Measure a spread to find the dome + interpolate #3 (O4.180).

## Step C — rule strategy
Cross-family contrast: line 25 (La-123, n=3) vs line 26 (Y/La-123, n=3) proves cation matters at fixed n ⇒ multi-factor interaction.
Domain knowledge for analogs (no close measured analog exists for #4,5,7 regardless):
- #4 O8…Tl1.800 (single-Tl Tl-1212, n=2): single-Tl-12(n-1)n series is lower than Hg/double-Tl at same n → Tl-1212 ~80 K → LOW
- #5 O7…Tl0.5Pb0.5 Sr2 (single Tl/Pb-1212, n=2, Sr): ~80–90 → LOW
- #6 O8Cu3Y1… (Y-123, n=3, Y-rich): ~92 K → HIGH
- #7 O8Cu4Y1… (Y-124, n=4): Y-12 family high only at n=3; n=4 ~80 → LOW

## Batch 1 results (6 calls, budget 16/18)
O4 series (Hg-1201, n=1) — candidates for Q1 (threshold1=95.0):
| O | Tc | >=95? |
|---|---|---|
| 4.047 | 95.0 | ✓ (=) |
| 4.097 | 95.3 | ✓ |
| 4.150 | 97.0 | ✓ |
| 4.200 | 94.2 | ✗ (below!) |
| 4.220 | 97.0 | ✓ |

Cross-family:
- O7Cu3Ba2La1 (La-123, n=3) = 89.6 → LOW (<90)
- O7Cu3Y0.200Ba2La0.800 (Y/La-123, n=3) = 81.0 → LOW (<90)
⇒ Y-12(n=3) family is LOW here (confirms Y-12 family NOT generically high).

## Batch 2 results (2 calls, budget 18/18 — EXHAUSTED)
- O4.220 = 97.0 (≥95 ✓) — high end of O4 dome stays high.
- O6Cu1Ba2Hg1Tl1 (Hg/Tl-1201, n=1, 5 el) = 0.0 — NOT superconducting.
  Anchor: a Tl-bearing n=1 (single layer, off ideal double-Tl stoichiometry) is INACTIVE.
  ⇒ "double-Tl layer active; single-Tl/deviant stoichiometry inactive" — n=2 analogs (#4,#5) → LOW.

## Step A/B summary
- T\* = 127.0 K; peak = `O6.220Ca1Cu2Ba2Hg1`; threshold1 = 95.0 K.

## Step B — Q1 (Hg ∧ exactly 4 elements ∧ Tc≥95.0)
Only the O4 (Hg-1201, n=1) family qualifies (5 elements: lines 22,24; no Hg: lines 23,25,26).
Measured O4 ≥95: {4.047(95.0), 4.097(95.3), 4.150(97.0), 4.220(97.0)}.
O4.200 = 94.2 (exclude).
Q1 = the 4 qualifying O4 members above.
(EXTRAPOLATED: 4.081, 4.085, 4.101, 4.103, 4.162 — all between measured highs; dome ~95–97 flat ⇒ all ≥95 ⇒ add to Q1.
EXTRAPOLATED excluded: 4.200(94.2 measured, below). No other O4 below 95 measured ⇒ no exclusion among unmeasured.)
⇒ Q1 (final) = {O4.047, O4.081, O4.085, O4.097, O4.101, O4.103, O4.150, O4.162, O4.220}Cu1Ba2Hg1 (9 members; O4.200 excluded).

## Step C — induced rule (multi-factor)
**Rule (from measured data + structural chemistry):** A composition is HIGH iff BOTH (a) its [Hg or double-Tl] superconducting block matches a structurally complete, stoichiometrically near-ideal double-layer M-12(n-1)n family with n ∈ {1,2,3}, AND (b) its oxygen content lies within that family's measured superconducting dome (not the under/over-doped, non-superconducting, or chemically disrupted regime).
- "single factor wrong": O4.200 (has Hg, n=1, 4 el) is LOW-ish (94.2<thr; near-low) — Hg alone not enough. O6Cu1Ba2Hg1Tl1 (n=2, has Tl) = 0 — layer identity/stoichiometry overrides. Y-12(n=3) family (lines 25,26) LOW despite n=3 — cation/site matters.
- ⇒ the interaction = [block type] × [n (Cu count)] × [cation family] × [O-doping regime], with two unmeasured interpolation points (#1,#2,#3) confined to *measured* O-domes.

## Predictions for all 7 unmeasurable
1. O6.160Ca1Cu2Ba2Hg1 — in O6 dome (between 105 & 122) → **high**
2. O6.270Ca1Cu2Ba2Hg1 — in O6 dome (between 123 & 100) → **high**
3. O4.180Cu1Ba2Hg1 — in O4 dome (between 97 & 94.2; ~95.6) → **high**
4. O8Ca0.850Cu2.200Ba2Tl1.800 — single-Tl Tl-1212 (n=2), deviant stoich → **low** (anchor: O6Cu1Ba2Hg1Tl1 = 0)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — single Tl/Pb-1212, Sr matrix → **low** (~80–90, below 90)
6. O8Cu3Y1Ba1.900La0.100 — Y-123 (n=3, Y-rich) → **low** (anchor: Y/La-123 = 81)
7. O8Cu4Y1Ba1.870La0.130 — Y-124 (n=4) → **low** (Y-12 high only at n=3; n=4 too low)

## Step D — final pick
Predicted-HIGH unmeasurable: #1(5 el), #2(5 el), #3(4 el), #6(5 el).
Fewest elements = **#3 O4.180Cu1Ba2Hg1 (4 elements)**.
(Tie-break not needed; #3 has 4 elements, all others have 5.)

## Q1 decision (strict no-guessing)
Step B = "report ALL whose Tc >= threshold1" + "never guess a measured number."
Candidates (Hg ∧ exactly 4 el) = 11 O4 members. I measured 5; 4 qualify (O4.047=95.0, O4.097=95.3, O4.150=97.0, O4.220=97.0); O4.200=94.2 excluded.
The 6 unmeasured O4 (4.081,4.085,4.096,4.101,4.103,4.162) are extrapolated-likely-≥95 (interior of flat 94–97 dome) but EXCLUDED from Q1 per the no-guessing rule (can't assert unmeasured Tc). Q1 = 4 measured-qualifying members.

## #6 reasoning (Y-123, n=3 → HIGH)
Task: rule is multi-factor interaction, single-factor rules fail "on several." Measured La-123(89.6) & Y0.2La0.8-123(81.0) are both LOW despite n=3 ⇒ n=3 alone fails ⇒ need Y (not La) at RE site. #6 = Y1Ba1.9La0.1 (Y-rich, n=3) ⇒ HIGH (canonical Y-123). #7 = Y-124 (n=4) ⇒ LOW (Y-12 high only at n=3). This is the cation×n interaction the task targets. (Step D = #3 regardless of #6, since #6 has 5 elements vs #3's 4.)

## Final answer committed
peak=O6.220Ca1Cu2Ba2Hg1, T*=127.0, threshold=95.0,
Q1=[O4.047,O4.097,O4.150,O4.220]Cu1Ba2Hg1,
predictions: #1 high, #2 high, #3 high, #4 low, #5 low, #6 high, #7 low,
final=O4.180Cu1Ba2Hg1.
