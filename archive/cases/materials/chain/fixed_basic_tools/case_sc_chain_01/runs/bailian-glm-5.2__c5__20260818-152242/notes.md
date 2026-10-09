# Superconductor Tc study — running notes

## Budget
- 18 measure_tc calls total. Must be strategic.

## Data
- measurable.txt: 27 entries
  - Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 (Hg-1212 series, same elements as seed, varying O) — 10 members
  - Lines 11-21: O4.xxxCu1Ba2Hg1 (Hg-1201 series, 4 elements O/Cu/Ba/Hg, contains Hg) — 11 members
  - Line 22: O6Cu1Ba2Hg1Tl1 (5 elem)
  - Line 23: O10Ca2Cu3Ba2Tl2 (Tl-2223)
  - Line 24: O6Cu1Cd0.250Ba2Tl1.750
  - Line 25: O7Cu3Ba2La1
  - Line 26: O7Cu3Y0.200Ba2La0.800 (YBCO-like)
- unmeasurable.txt: 7 entries
  1. O6.160Ca1Cu2Ba2Hg1  (Hg-1212 series — interpolate from Step A)
  2. O6.270Ca1Cu2Ba2Hg1  (Hg-1212 series — interpolate from Step A)
  3. O4.180Cu1Ba2Hg1     (Hg-1201 series — interpolate from Step B)
  4. O8Ca0.850Cu2.200Ba2Tl1.800  (Tl-1212-like, Cu2)
  5. O7Ca1Cu2Sr2Tl0.500Pb0.500  (Tl/Pb-1212-like, Cu2, Sr)
  6. O8Cu3Y1Ba1.900La0.100      (YBCO-123-like, Cu3)
  7. O8Cu4Y1Ba1.870La0.130      (Y-124-like, Cu4)

## Plan
- Step A: measure all 10 Hg-1212 series members → T* = max. (10 calls)
- Step B: threshold1 = T* - 32. Find Q1 = 4-element Hg materials with Tc >= threshold1. Measure Hg-1201 strategically (near-optimal + neighbors of O4.180).
- Step C: induce multi-factor rule from measured materials; predict high/low for 7 unmeasurable.
- Step D: among predicted-high unmeasurable, fewest elements (tie-break higher expected Tc).

## Step A results (Hg-1212 series, O6.xCa1Cu2Ba2Hg1)
| O | Tc |
|---|---|
| 6.080 | 92.0 |
| 6.100 | 92.5 |
| 6.130 | 102.0 |
| 6.140 | 105.0 |
| 6.190 | 122.0 |
| 6.210 | **127.0** ← peak (tie) |
| 6.220 | **127.0** ← peak (tie) |
| 6.240 | 123.0 |
| 6.300 | 100.0 |
| 6.310 | 98.0 |

T* = 127.0. Peak composition: O6.210Ca1Cu2Ba2Hg1 (first to reach max; tie with O6.220).
Curve is unimodal, peak plateau at x≈6.21–6.22. For interpolation (Step C, unmeasurables O6.160 & O6.270):
- O6.160 between 6.140(105) and 6.190(122): interpolate ~113 K → HIGH
- O6.270 between 6.240(123) and 6.300(100): interpolate ~111 K → HIGH
Both clearly ≥90 → HIGH.

## Step B setup
threshold1 = T* - 32 = 127 - 32 = 95.0
Filter: contains Hg AND exactly 4 elements.
- Hg-1212 (O,Ca,Cu,Ba,Hg)=5 elem → NO
- Hg-1201 (O4.xCu1Ba2Hg1) = (O,Cu,Ba,Hg)=4 elem, has Hg → YES (11 members)
- O6Cu1Ba2Hg1Tl1 = 5 elem → NO
- others: no Hg → NO
So Q1 ⊆ Hg-1201 series. Need Tc>=95.0 among:
4.047, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.162, 4.200, 4.220
Sorted by δ. Real Hg-1201 peaks ~94-97 K near δ≈0.10 → threshold 95 is near peak.
Budget after Step A: 8 left.

## Step B measurements (Hg-1201, O4.xCu1Ba2Hg1) — 8 measured, 3 unmeasured
| O | Tc | ≥95? |
|---|---|---|
| 4.047 | 95.0 | YES (=) |
| 4.081 | 98.0 | YES |
| 4.085 | — | (not measured) |
| 4.096 | 95.6 | YES |
| 4.097 | 95.3 | YES |
| 4.101 | 93.4 | NO |
| 4.103 | — | (not measured) |
| 4.150 | 97.0 | YES |
| 4.162 | — | (not measured) |
| 4.200 | 94.2 | NO |
| 4.220 | 97.0 | YES |

Not measured (no budget): O4.085, O4.103, O4.162.
- O4.085 between 4.081(98) and 4.096(95.6): interp ~96.8 → ≥95 → YES (qualifies)
- O4.103 between 4.101(93.4) and 4.150(97.0): interp ~94.3 → <95 → NO. But real curve is non-monotonic? Let me think: 4.097=95.3, 4.101=93.4 (drop), 4.150=97.0 (rise). Odd. The series is noisy/oscillatory around ~94-97. O4.103 just above 4.101(93.4): likely ~93-94 → NO.
- O4.162 between 4.150(97.0) and 4.200(94.2): interp ~96.0 → ≥95 → YES (qualifies)

threshold1 = 95.0 (>= means ≥).

Q1 (measured ≥95.0): O4.047Cu1Ba2Hg1 (95.0), O4.081Cu1Ba2Hg1 (98.0), O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3), O4.150Cu1Ba2Hg1 (97.0), O4.220Cu1Ba2Hg1 (97.0).
Q1 (inferred ≥95.0): O4.085 (~96.8), O4.162 (~96.0).

NOTE on Q1 membership: task says "report ALL whose Tc >= threshold1". I could only measure 8 of 11 Hg-1201 members (budget). The 3 unmeasured are interpolated:
- O4.085 ≈ 96.8 ≥ 95 → include
- O4.103 ≈ 93-94 < 95 → exclude
- O4.162 ≈ 96.0 ≥ 95 → include

Full Q1 = {O4.047, O4.081, O4.085(inf), O4.096, O4.097, O4.150, O4.162(inf), O4.220}Cu1Ba2Hg1.
For answer, list the confidently-qualifying ones. I will report the full inferred set but flag inference for the 3. Given "use measure_tc for every Tc value you cite — never guess a measured number": Q1 is a set, not Tc numbers. The membership for the 3 unmeasured is inferred, not measured. Safest: report Q1 as the 6 measured-qualifying members, and note the 3 unmeasured Hg-1201 are excluded from a measured-only Q1 (cannot confirm membership without measurement). Actually — re-reading: "report ALL whose Tc >= threshold1". The honest answer with my budget: I can only confirm 6. I'll report the 6 measured-qualifying and document the 3 I couldn't measure.

Decision: Q1 = the 6 measured-qualifying members. Document unmeasured Hg-1201 in notes.

## Step C — rule induction + predictions
Budget exhausted (18/18). All 18 measured materials are Hg-cuprates (Hg-1212 + Hg-1201), ALL HIGH (Tc 92–127). So the rule is anchored by measurement on Hg series, extended to the Tl/YBCO unmeasurables via cuprate physics (the task explicitly says single-factor rules fail and the rule is a multi-factor interaction).

### Measured anchors
- Hg-1212 (O6.08–6.31, 5 elem O/Ca/Cu/Ba/Hg): HIGH across the whole doping range (92–127 K), peak ~127 K at O6.21–6.22.
- Hg-1201 (O4.05–4.22, 4 elem O/Cu/Ba/Hg): HIGH across range (93–98 K).

### Multi-factor rule (interaction)
HIGH Tc (≥90 K) requires a layered cuprate that jointly has:
 (i) an optimal Cu–O plane block (1–3 Cu layers) paired with the matching heavy-cation charge-reservoir layer (Ba2 + Hg/Tl + Ca/Y spacer) — i.e. a known high-Tc structure type (Hg-1201, Hg-1212, Tl-2212, optimally-doped Y-123), AND
 (ii) oxygen/hole doping inside that structure's optimal plateau.
Single factors fail: "contains Hg/Tl", "Cu2", "YBCO family" each misclassify — overdoped YBa2Cu3O8 (Y-123 at O8) is LOW despite the famous family, and single-(Tl,Pb)-layer 1212 with Sr2 (Cu2) is LOW despite Cu2+Tl.

### Predictions for the 7 unmeasurables
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212; between measured O6.140(105) & O6.190(122) → ~113 K → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212; between O6.240(123) & O6.300(100) → ~111 K → **HIGH**
3. O4.180Cu1Ba2Hg1 — Hg-1201; between O4.150(97) & O4.200(94.2) → ~96 K → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl≈2 (Tl1.8) ⇒ double-Tl 2212 block (Tl2Ba2CaCu2O8, ~108 K), Cu2, Ba2, Ca0.85 → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — single (Tl,Pb) layer (0.5+0.5=1), Cu2, Sr2 ⇒ (Tl,Pb)-1212 with Sr ~80 K → **LOW**
6. O8Cu3Y1Ba1.900La0.100 — Y-123 family but O8 = severely overdoped (O7→92 K; O8 suppressed), +La holes ⇒ overdoped → **LOW**
7. O8Cu4Y1Ba1.870La0.130 — Y-124 (YBa2Cu4O8) ~80 K intrinsically (underdoped), +La holes still <90 → **LOW**

Predicted HIGH: {#1, #2, #3, #4}. Predicted LOW: {#5, #6, #7}.

## Step D — final pick (among predicted HIGH, fewest elements; tie-break higher expected Tc)
Element counts of predicted-HIGH:
- #1 O6.160Ca1Cu2Ba2Hg1: O,Ca,Cu,Ba,Hg = 5
- #2 O6.270Ca1Cu2Ba2Hg1: 5
- #3 O4.180Cu1Ba2Hg1: O,Cu,Ba,Hg = **4** ← fewest
- #4 O8Ca0.850Cu2.200Ba2Tl1.800: O,Ca,Cu,Ba,Tl = 5
#3 is the unique 4-element high → **final = O4.180Cu1Ba2Hg1** (expected Tc ~96 K).
ROBUSTNESS: #3 is interpolated from measured Hg-1201 neighbors (97 & 94.2) ⇒ ~96 ≥ 90 → solidly HIGH; and it is the only ≤4-element composition in the unmeasurable list, so it is the fewest-element high regardless of the (physics-based) #4–#7 calls. Final is insensitive to the uncertain Tl/YBCO predictions.

## ANSWER
- peak: "O6.210Ca1Cu2Ba2Hg1" (T* = 127.0; ties with O6.220 at 127.0)
- peak_tc: 127.0
- threshold: 95.0
- qualifying (Q1, measured ≥95.0 among 4-element Hg = Hg-1201): O4.047, O4.081, O4.096, O4.097, O4.150, O4.220 (×Cu1Ba2Hg1). [3 Hg-1201 members — O4.085, O4.103, O4.162 — were not measured due to budget.]
- predictions: 7 entries (see above)
- final: "O4.180Cu1Ba2Hg1"
- rule: multi-factor interaction (see above)
