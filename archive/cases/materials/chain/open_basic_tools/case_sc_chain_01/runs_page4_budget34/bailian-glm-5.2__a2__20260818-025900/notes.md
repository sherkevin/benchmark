# Superconductor Tc Study — Notes

## Task overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg → 5 elements; oxygen ~6.08)
- High = Tc >= 90 K, Low = Tc < 90 K
- Steps:
  - A: Find series members (same element set, varying O), measure, find MAX Tc = T*
  - B: threshold1 = T* - 32; find Hg-containing materials with exactly 4 elements and Tc >= threshold1 → Q1
  - C: Predict high/low for unmeasurable materials (measure_tc returns "no sample exists")
  - D: Among predicted-high unmeasurable, pick FEWEST elements (tie-break higher expected Tc)

## Budget tracker
Start: 34

## Step A — series (O_x Ca1Cu2Ba2Hg1, 5 elements, varying O) — 12 members
Measured Tc:
- O6.080 → 92.0
- O6.100 → 92.5
- O6.130 → 102.0
- O6.140 → 105.0
- O6.160 → NO SAMPLE (unmeasurable)
- O6.190 → 122.0
- O6.210 → 127.0  ★ T* (peak)
- O6.220 → 127.0  ★ (tie peak)
- O6.240 → 123.0
- O6.270 → NO SAMPLE (unmeasurable)
- O6.300 → 100.0
- O6.310 → 98.0

**T* = 127.0 (at O6.210 and O6.220)**
**threshold1 = T* - 32 = 95.0**

## Full Hg list (27 materials, all found)
O4-series (O,Cu,Ba,Hg = 4 elements) — 14 members, candidates for Step B:
- O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103 (low cluster)
- O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 (high cluster)
O6-series (5 elem, Ca) — 12 members (above)
O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl) — excluded from B

## Non-Hg materials also discovered (via Ca/Ba search)
- O10Ca2Cu3Ba2Tl2 (O,Ca,Cu,Ba,Tl = 5 elem)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (O,Ca,Cu,Sr,Tl,Pb = 6 elem)
- O8Ca0.850Cu2.200Ba2Tl1.800 (O,Ca,Cu,Ba,Tl = 5 elem)
(No Hg → not in Step B. Won't win Step D vs 4-elem unmeasurable.)

## Step B plan
4-element Hg = O4-series (14). threshold1 = 95.0. Find all with measured Tc >= 95.0.
Budget after searches: 22 used, 12 left. Measure 12 of 14 O4 (skip O4.047, O4.066 — most underdoped, lowest Tc, least likely >= 95; if unmeasurable they'd be predicted low, don't affect Step D).

## Step C/D plan
Unmeasurable so far: O6.160, O6.270 (5 elem). Measure O4-series to find 4-elem unmeasurable (would win Step D if predicted high).

## Step B — O4-series (O,Cu,Ba,Hg = 4 elements, 14 members) full data
Measured Tc:
- O4.047 → NO SAMPLE (unmeasurable)
- O4.066 → NO SAMPLE (unmeasurable)
- O4.081 → 98.0
- O4.085 → 92.0
- O4.096 → 95.6
- O4.097 → 95.3
- O4.101 → 93.4
- O4.103 → 92.5
- O4.150 → 97.0
- O4.160 → NO SAMPLE (unmeasurable)
- O4.162 → 97.4
- O4.180 → NO SAMPLE (unmeasurable)
- O4.200 → 94.2
- O4.220 → 97.0

## Other Hg material
- O6Cu1Ba2Hg1Tl1 → 0.0 (measurable, 5 elem, no Ca; out of Step B which needs exactly 4 elem)

## Complete unmeasurable list (from Hg searches) — 6 materials
1. O6.160Ca1Cu2Ba2Hg1 (O6 series, 5 elem) — O=6.160
2. O6.270Ca1Cu2Ba2Hg1 (O6 series, 5 elem) — O=6.270
3. O4.047Cu1Ba2Hg1 (O4 series, 4 elem) — O=4.047
4. O4.066Cu1Ba2Hg1 (O4 series, 4 elem) — O=4.066
5. O4.160Cu1Ba2Hg1 (O4 series, 4 elem) — O=4.160
6. O4.180Cu1Ba2Hg1 (O4 series, 4 elem) — O=4.180

## Step B — Q1 (Hg, exactly 4 elem, measured Tc >= threshold1=95.0)
Sort all measured O4 by Tc desc:
- O4.081 → 98.0  ✓ (>=95)
- O4.162 → 97.4  ✓
- O4.150 → 97.0  ✓
- O4.220 → 97.0  ✓
- O4.096 → 95.6  ✓
- O4.097 → 95.3  ✓
- O4.101 → 93.4  ✗
- O4.085 → 92.0  ✗
- O4.103 → 92.5  ✗
- O4.200 → 94.2  ✗
Unmeasurable O4 (O4.047, O4.066, O4.160, O4.180) — not synthesized, excluded from Q1.
**Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]**

## Step C — rule induction
Training data (all Hg-family measured):
- O6 family Ca1Cu2Ba2Hg1 (Hg-1212): O 6.080–6.310, ALL Tc 92–127 → high across window. Peak 127 @ O6.21–6.22.
- O4 family Cu1Ba2Hg1 (Hg-1201): O 4.081–4.220, ALL Tc 92–98 → high across window. Max 98 @ O4.081.
- O6Cu1Ba2Hg1Tl1 (Hg site partly Tl?): Tc = 0.0 → LOW. (Tl/Pb on Hg site kills superconductivity.)

Key insight (why single-factor fails): O6Cu1Ba2Hg1Tl1 has high O (6) and is a Hg-Ba-Cu cuprate yet Tc=0. An O-content rule OR a "contains Hg+Cu+Ba" rule would wrongly call it high. The deciding factors are (1) no Tl/Pb substituting for Hg, and (2) O within the family's superconducting doping window. Element-count alone also fails: O4.160/O4.180 (4 elem, no Ca) are high; Ca-presence alone fails for the same reason.

Rule: "High iff the material is a Tl/Pb-free Hg–Ba–Cu(-Ca) cuprate whose oxygen content lies within its family's measured superconducting window (Hg-1212: O≈6.08–6.31; Hg-1201: O≈4.08–4.22). Hg-site substitution by Tl/Pb → ~0 K (low). Members well outside the window on the underdoped side drop below 90 K (low)."

## Predictions for the 6 unmeasurable Hg materials
- O6.160Ca1Cu2Ba2Hg1 → HIGH (O=6.160 inside 6.08–6.31 window; between 6.14→105 and 6.19→122 → ~113)
- O6.270Ca1Cu2Ba2Hg1 → HIGH (O=6.270 inside window; between 6.24→123 and 6.30→100 → ~112)
- O4.047Cu1Ba2Hg1  → LOW  (O=4.047, below 4.081 floor of Hg-1201 window; heavily underdoped → <90)
- O4.066Cu1Ba2Hg1  → LOW  (O=4.066, below 4.081 floor; underdoped leg → <90)
- O4.160Cu1Ba2Hg1  → HIGH (O=4.160 inside 4.08–4.22 window; between 4.150→97.0 & 4.162→97.4 → ~97.2)
- O4.180Cu1Ba2Hg1  → HIGH (O=4.180 inside window; between 4.162→97.4 & 4.200→94.2 → ~95.8)

## Step D — final pick (among predicted-HIGH unmeasurable, fewest elements; tie→higher expected Tc)
Predicted-high candidates:
- 4 elements: O4.160 (~97.2), O4.180 (~95.8)
- 5 elements: O6.160 (~113), O6.270 (~112)
Fewest elements = 4 → {O4.160, O4.180}. Tie-break higher expected Tc → O4.160 (~97.2 > ~95.8).
**FINAL = O4.160Cu1Ba2Hg1**

## Scope note
Discovered (via Ca/Ba search pages) but NOT confirmed unmeasurable, excluded from predictions: O10Ca2Cu2Ba2Tl2, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800 (all Tl/Pb-bearing, no Hg → out of Hg-family scope; by the rule they'd be LOW if unsynthesized, and with 5–6 elements they can't win Step D). Full Hg list (27) confirmed exhausted (page 7 has_more=false).

## Budget summary
Used 33/34: 7 search pages + 26 measurements. (1 unit held in reserve — no remaining measurement could verify an unmeasurable prediction by definition.)
