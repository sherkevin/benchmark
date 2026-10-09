# Superconductor Tc Study — Notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg)
- High = Tc >= 90 K, Low = Tc < 90 K
- Budget: 26 units total (shared by measure_tc and search_db)
- Step A: find series peak (same elements, varying O) → T*
- Step B: threshold1 = T* - 32; Q1 = Hg-containing materials with exactly 4 elements, Tc >= threshold1
- Step C: predict high/low for unmeasurable materials
- Step D: among predicted-high unmeasurable, pick fewest elements (tie → higher expected Tc)

## Budget tracker
- Used: 26/26 EXHAUSTED (19 search pages + 7 measures)
- Remaining: 0

## Searches
- "Hg" pages 1-5: O4-series (O,Cu,Ba,Hg = 4 elem) — O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 (14 so far) + seed O6.080Ca1Cu2Ba2Hg1. 27 total Hg matches → pages 6-9 still unsearched (12 more matches).
- "Ca" pages 1-5 (15 matches, done): seed series O6.xxxCa1Cu2Ba2Hg1 members = O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 (12 members). Also O10Ca2Cu3Ba2Tl2, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800.

## Step A — seed series (O6.xxxCa1Cu2Ba2Hg1, 5 elements: O,Ca,Cu,Ba,Hg)
Measurements:
- O6.080 → 92.0
- O6.100 → 92.5
- O6.140 → 105.0
- O6.160 → NO SAMPLE (unmeasurable)
- O6.190 → 122.0
- O6.220 → 127.0  ← peak so far
- O6.240 → 123.0
- O6.270 → NO SAMPLE (unmeasurable)
- O6.300 → 100.0
- (not measured: O6.130, O6.210, O6.310)

Peak observed: O6.220 → 127.0K. Candidates 130-160, 210-260 around it: measured O6.190=122, O6.240=123 (both below 127). O6.220 appears to be the peak T* = 127.0.
Parabola fit (0.190,122),(0.220,127),(0.240,123) → peak δ≈0.216, T≈127.1K → confirms O6.220 (δ=0.22) is the peak. T* = 127.0.
threshold1 = 127 - 32 = 95.0

## Step B — Q1 (Hg + exactly 4 elements, Tc >= 95)
4-element Hg materials = O4.xxxCu1Ba2Hg1 series (O,Cu,Ba,Hg) — Hg-1201 analog, real peak ~96-97K at δ≈0.16-0.20. threshold 95.0.
Could not measure (budget exhausted). Estimate from real Hg-1201 curve: Tc>=95 for δ∈[~0.09,~0.22].
Q1 (estimate) = O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 (all Cu1Ba2Hg1). Excludes O4.047,O4.066 (δ too low) and O4.081,O4.085 (borderline <95).

## Step C — unmeasurable predictions (5 found)
- O4.180Cu1Ba2Hg1 (4 elem, δ=0.18 near Hg-1201 peak) → HIGH (~96-97K, interp O4.16-4.20 peak)
- O6.160Ca1Cu2Ba2Hg1 (5 elem, δ=0.16, between O6.140=105 and O6.190=122) → HIGH (~113K)
- O6.270Ca1Cu2Ba2Hg1 (5 elem, δ=0.27, between O6.240=123 and O6.300=100) → HIGH (~112K)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb) — Tl-1212 w/ Pb+Sr substitution, reduced Tl → LOW (<90K)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl) — Tl-1212 w/ Ca deficient (0.85), Tl deficient (1.8), Cu excess → LOW (<90K)

Rule (multi-factor): Tc depends on (1) homologous series + oxygen δ (each Hg-cuprate series has optimal-δ peak: Hg-1201 ~97K, Hg-1212 ~127K, falling off under/over-doped) AND (2) cation stoichiometry — Ca/Tl deficiency or Pb/Sr substitution suppresses Tc. Single-factor "Tl present→high" is WRONG: deficient/substituted Tl-1212 materials are LOW.

## Step D — final
Predicted HIGH unmeasurable: O4.180 (4 elem), O6.160 (5 elem), O6.270 (5 elem).
Fewest elements = O4.180Cu1Ba2Hg1 (4 elements). FINAL = O4.180Cu1Ba2Hg1.

## Budget final
- Used: 26/26 (19 search pages + 7 measures). Budget exhausted before O4 series measured.
- Note: 6 materials (35 total - 29 found) not discovered; 2 more unmeasurable materials exist but not found. Predictions cover the 5 unmeasurable materials discovered.

## Step B candidate materials (Hg + exactly 4 elements)
- O4.xxxCu1Ba2Hg1 series (O,Cu,Ba,Hg = 4 elem) — Hg-1201 analog. Real peak ~97K near O4.16.

