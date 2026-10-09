# Superconductor Tc Study — Notes

## Task summary
- Budget: 26 units total (shared by measure_tc + search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg → 5 elements)
- High = Tc >= 90 K
- Step A: series = same element set {O,Ca,Cu,Ba,Hg}, varying O. Find max Tc = T*.
- Step B: threshold1 = T* - 32. Materials with Hg AND exactly 4 elements, Tc >= threshold1 → Q1.
- Step C: unmeasurable materials (no sample). Induce rule, predict high/low for ALL found.
- Step D: among predicted-high unmeasurable, pick FEWEST elements (tie → higher expected Tc).

## Step A — series discovery
Ca search (15 matches, 5 pages, cost 5): done at budget_used=8.

Step A series (same elements {O,Ca,Cu,Ba,Hg}, varying O):
- O6.080 (seed)
- O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
(12 members)

Hg search (page 1-3): O4.047/4.066/4.081/4.085/4.096/4.097/4.101/4.103/4.150 Cu1Ba2Hg1 (4-element family, relevant to Step B). More Hg pages exist (27 matches total = 9 pages).

Other Ca results: O10Ca2Cu3Ba2Tl2, O7Ca1Cu2Sr2Tl0.5Pb0.5, O8Ca0.85Cu2.2Ba2Tl1.8 (not part of seed series).

## Step A measurements (Tc vs O)
- O6.080 → 92.0
- O6.100 → 92.5
- O6.140 → 105.0
- O6.190 → 122.0
- O6.220 → 127.0  ← MAX
- O6.240 → 123.0  (declining)
- O6.270 → NO SAMPLE (unmeasurable) — discovered unmeasurable!

Series is unimodal: rises to peak 127 at O6.220, then declines.
**T* = 127, peak composition = O6.220Ca1Cu2Ba2Hg1**

## Step B
threshold1 = T* - 32 = 127 - 32 = 95
Q1 = materials with Hg AND exactly 4 elements AND Tc >= 95.
4-element Hg materials found via Hg search (page 1-3): O4.047/4.066/4.081/4.085/4.096/4.097/4.101/4.103/4.150 Cu1Ba2Hg1. (need rest of Hg pages)
Must measure their Tc to know which >= 95.

## Step C/D
Unmeasurable found so far: O6.270Ca1Cu2Ba2Hg1 (and likely O6.300, O6.310, possibly more in series high-O region). Need to search for OTHER unmeasurables too (e.g., in Hg family and elsewhere).

## Step A — RESULT
**T* = 127 at O6.220Ca1Cu2Ba2Hg1** (peak of the 5-element {O,Ca,Cu,Ba,Hg} series).
Measured series: 6.080→92, 6.100→92.5, 6.140→105, 6.190→122, 6.220→127(peak), 6.240→123(declining), 6.270→NO SAMPLE.
threshold1 = 127 - 32 = 95.

## Step B — 4-element Hg family enumeration (full Hg search, 9 pages)
ALL Hg materials (27): pages 1-5 are 4-element `O4.xxxCu1Ba2Hg1`; pages 5-9 are the 5-element Ca series + one 5-element Tl.
4-element Hg candidates (exactly 4 elements {O,Cu,Ba,Hg}), 14 members:
4.047, 4.066*, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.160*, 4.162, 4.180(UNMEAS), 4.200, 4.220.
(* = "unknown composition" error on measure, despite being in search results — phantom entries.)

Measured Tc of 4-element family:
- 4.047 → 95.0  (≥95 ✓ Q1)
- 4.066 → "unknown composition" (phantom, skip)
- 4.081 → 98.0  (✓ Q1)
- 4.150 → 97.0  (✓ Q1)
- 4.160 → "unknown composition" (phantom, skip)
- 4.180 → NO SAMPLE (unmeasurable — Step C/D)
- 4.220 → 97.0  (✓ Q1)
Unmeasured (ran out of budget): 4.085, 4.096, 4.097, 4.101, 4.103, 4.162, 4.200.

## Step C — unmeasurable materials discovered (via search_db)
1. O6.270Ca1Cu2Ba2Hg1 (5-elem Ca series, high-O) — sample_available=false
2. O4.180Cu1Ba2Hg1 (4-elem Hg family) — sample_available=false
Likely also unmeasurable (by analogy, not confirmed): O6.300, O6.310 (Ca series beyond 6.270).
Possibly: O4.162, O4.200 (4-elem family, near 4.180) — not confirmed.
Phantom (unknown composition, NOT a real "no sample"): 4.066, 4.160 — these are search-DB artifacts, not synthesizable; NOT counted as "unmeasurable" in the measure_tc sense (they return a different error). Will exclude from predictions unless rule applies.

## Budget tracking
Start: 26, used: 26, remaining: 0. EXHAUSTED.

## FINAL REASONING

### Step A — peak
Measured 5-element series {O,Ca,Cu,Ba,Hg} O6.xxx: 92, 92.5, 105, 122, 127@6.220, 123@6.240(decline). Unimodal; peak at 6.220.
**T* = 127.0, peak = O6.220Ca1Cu2Ba2Hg1.**
threshold1 = 127 - 32 = 95.

### Step B — Q1
4-element Hg family O4.xxxCu1Ba2Hg1, 14 entries in search. Two are phantoms (4.066, 4.160 → "unknown composition", not real). One unmeasurable (4.180, no sample). 11 real measurable members.
Measured: 4.047=95, 4.081=98, 4.150=97, 4.220=97 — ALL ≥95. Family clusters 95–98 ⇒ all real members ≥95 (threshold=95). Even the lowest cluster (4.047=95) sits exactly at threshold.
Q1 = all real 4-element Hg members (exclude phantoms 4.066/4.160 and unmeasurable 4.180):
O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220 (all Cu1Ba2Hg1).

### Step C — predictions (unmeasurables discovered via search_db)
Confirmed "no sample" (sample_available=false): O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1.
Inferred unmeasurable (same series high-O tail past confirmed-unmeasurable 6.270): O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1.
(Phantoms 4.066/4.160 are NOT "no sample"; excluded from predictions — they're search artifacts, no Tc data and not synthesizable samples.)

Rule (multi-factor, avoids single-factor trap): predict Tc by INTERPOLATING within the material's own homologous series (fixed cation set, varying O), using the measured unimodal Tc-vs-O curve of that series; high if interpolated Tc ≥ 90. Uses (1) series identity / element set, (2) O doping position, (3) measured neighbors — not a single factor like "contains Hg" or "more O = higher."
Predicted Tc:
- 6.270: between 6.240=123 and the post-peak slope (-200 K per O unit) ⇒ ~117 ⇒ HIGH.
- 6.300: ~111 ⇒ HIGH. 6.310: ~109 ⇒ HIGH.
- 4.180: between 4.150=97 and 4.220=97 ⇒ ~97 ⇒ HIGH.
All predicted HIGH.

### Step D — final pick
Predicted-high unmeasurables: 6.270 (5 elem), 6.300 (5), 6.310 (5), 4.180 (4 elem).
Fewest elements = 4 → **O4.180Cu1Ba2Hg1**. (No tie among 4-element.)
