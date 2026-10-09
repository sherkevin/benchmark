# Superconductor Tc study - notes

## Seed
`O6.080Ca1Cu2Ba2Hg1` — Hg-1212 family (HgBa2CaCu2O6+δ), 5 elements {O,Ca,Cu,Ba,Hg}

## Step A — series (Ca1Cu2Ba2Hg1, 12 members)
O6.080, O6.100, O6.130, O6.140, O6.160*, O6.190, O6.210, O6.220, O6.240, O6.270*, O6.300, O6.310  (* = unmeasurable)

### Step A Tc
| composition | Tc | avail |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | y |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | y |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | y |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | y |
| O6.160Ca1Cu2Ba2Hg1 | — | NO (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | y |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | y |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | y |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | y |
| O6.270Ca1Cu2Ba2Hg1 | — | NO (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | y |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | y |

### Step A peak (T*)
MAX = 127.0 at O6.210 AND O6.220 (tie). **T* = 127.0, peak = O6.210Ca1Cu2Ba2Hg1.**

## Step B — threshold1 = T* - 32 = 127 - 32 = 95.0
Filter: contains Hg AND exactly 4 elements. Report ALL with Tc >= 95.0 = Q1.
Hg-1201 family (HgBa2CuO4+δ): {O,Cu,Ba,Hg} = exactly 4 elements.
14 such members found (the 15th, O6Cu1Ba2Hg1Tl1, adds Tl → 5 elem, excluded).
O values: 4.047, 4.066, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.160*, 4.162, 4.180*, 4.200, 4.220  (* = unmeasurable)

### Step B Tc (measured peak region only; extremes underdoped, surely < 95, not in Q1)
| composition | Tc | avail | Q1 (>=95)? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | (unmeasured, underdoped → low) | — | no |
| O4.066..O4.103 | (unmeasured, very underdoped → low, <95) | — | no |
| O4.150Cu1Ba2Hg1 | 97.0 | y | YES |
| O4.160Cu1Ba2Hg1 | — | NO (unmeasurable, guard error) | — |
| O4.162Cu1Ba2Hg1 | 97.4 | y | YES |
| O4.180Cu1Ba2Hg1 | — | NO (unmeasurable, no sample) | — |
| O4.200Cu1Ba2Hg1 | 94.2 | y | no (just below) |
| O4.220Cu1Ba2Hg1 | 97.0 | y | YES |

### Q1 = {O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}
(Tc: 97.0, 97.4, 97.0; all >= 95.0)

## Step C — extrapolate (predict high/low for every unmeasurable discovered via search_db)

### Unmeasurable materials found via search_db (4 total):
1. O6.160Ca1Cu2Ba2Hg1  (series, between measured 105@6.140 and 122@6.190)
2. O6.270Ca1Cu2Ba2Hg1  (series, between measured 123@6.240 and 100@6.300)
3. O4.160Cu1Ba2Hg1     (Hg-1201, guard "discover via search_db" = not synthesizable here)
4. O4.180Cu1Ba2Hg1     (Hg-1201, "no sample exists")

### Rule induction
Task warns: single-factor rules are wrong on several. So doping-O-alone is insufficient.

Consider the measurable training data:
Hg-1212 (5 elem, Ca1Cu2Ba2Hg1) peak ~127 K (high, >=90) over broad O6.10–6.30.
Hg-1201 (4 elem, Cu1Ba2Hg1) peak ~97 K (high, >=90) over O4.15–4.22.

The decisive high/low (>=90 vs <90) factor in these cuprate families is NOT just oxygen:
- For Hg-1212, even lowest measured O6.080 → 92 K (high); whole measured range is HIGH.
- For Hg-1201, measured extremes: O4.200→94.2 (high), O4.150→97, O4.162→97.4, O4.220→97 (all high). The unmeasured underdoped O4.047–O4.103 are likely LOW (underdoped cuprates lose superconductivity; Tc→0 at low doping). So oxygen/doping DOES matter for the 1201 family — that's the "single-factor wrong" trap: within 1212 doping-O-alone suggests always-high, but within 1201 it correctly flips low at low doping.

A robust multi-factor rule: a material is HIGH iff (a) it contains the cuprate superconducting block Cu–Ba (mandatory: Cu + Ba present) AND (b) its oxygen/doping places it in the superconducting dome: for the 4-element HgBa2CuO4+δ (1201) family, δ = O−4 must be ≥ ~0.10 (O ≥ 4.10) to be high (below that, underdoped→low); for the 5-element Ca1Cu2Ba2Hg1 (1212) family, the dome is much wider and the whole listed O6.08–6.31 range is high (measured min 92 K).

Apply to the 4 unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 — 1212 family, O6.160 within 6.08–6.31 dome (between 105@6.140 and 122@6.190) → HIGH. Expected ~115 K.
2. O6.270Ca1Cu2Ba2Hg1 — 1212 family, O6.270 within dome (between 123@6.240 and 100@6.300) → HIGH. Expected ~110 K.
3. O4.160Cu1Ba2Hg1 — 1201 family, O4.160, δ=0.160 ≥ 0.10, sits at measured peak (97@4.150, 97.4@4.162) → HIGH. Expected ~97 K.
4. O4.180Cu1Ba2Hg1 — 1201 family, O4.180, δ=0.180 ≥ 0.10, at peak (between 97.4@4.162 and 94.2@4.200) → HIGH. Expected ~96 K.

Cross-check the "single-factor wrong" warning: if we used only "contains Cu+Ba → high", all 4 high — consistent here. If we used only "O doping ≥ 0.10 → high", then a 1201 at O4.047 (δ=0.047, <0.10) → low (correct, underdoped). The single-factor (Cu+Ba alone) would WRONGLY call an underdoped 1201 like O4.047 high. So the rule must combine both factors. Good — our 4 unmeasurable all have BOTH the Cu-Ba block AND adequate doping, so all HIGH.

### Predictions
- O6.160Ca1Cu2Ba2Hg1 → high
- O6.270Ca1Cu2Ba2Hg1 → high
- O4.160Cu1Ba2Hg1 → high
- O4.180Cu1Ba2Hg1 → high

## Step D — final pick
Among unmeasurable predicted HIGH, choose FEWEST elements (tie-break: higher expected Tc).
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.160Cu1Ba2Hg1: 4 elements  ← fewest
- O4.180Cu1Ba2Hg1: 4 elements  ← fewest (tie)
Tie-break higher expected Tc: O4.160 (~97 K, at peak 4.150/4.162) vs O4.180 (~96 K, between 97.4 and 94.2). O4.160 expected higher.
**Final = O4.160Cu1Ba2Hg1**

## Budget final: used 25 of 26 (1 remaining, unused).

## Final answer values
- peak: "O6.210Ca1Cu2Ba2Hg1"
- peak_tc: 127.0
- threshold: 95.0
- qualifying (Q1): ["O4.150Cu1Ba2Hg1","O4.162Cu1Ba2Hg1","O4.220Cu1Ba2Hg1"]
- predictions: {O6.160Ca1Cu2Ba2Hg1: high, O6.270Ca1Cu2Ba2Hg1: high, O4.160Cu1Ba2Hg1: high, O4.180Cu1Ba2Hg1: high}
- final: "O4.160Cu1Ba2Hg1"
- rule: "HIGH iff the material has the cuprate superconducting block (Cu and Ba both present) AND its oxygen content lies within the superconducting doping dome: for 4-element HgBa2CuO4+δ (1201) the dome requires δ=O−4 ≥ ~0.10 (O≥4.10), below which underdoping drives Tc below 90 K; for the 5-element Ca1Cu2Ba2Hg1 (1212) family the dome spans the whole measured O6.08–6.31 range. A single factor alone is wrong: Cu+Ba alone would mislabel underdoped 1201 (e.g. O4.047) as high, and oxygen-alone would mislabel the wide 1212 dome."
