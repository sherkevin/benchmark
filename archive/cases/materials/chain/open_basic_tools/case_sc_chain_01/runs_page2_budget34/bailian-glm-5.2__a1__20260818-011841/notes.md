# Superconductor Tc Study — Running Notes

Seed: `O6.080Ca1Cu2Ba2Hg1`
Budget: 34 units total (shared between measure_tc and search_db)
High = Tc >= 90 K

## Step A — series peak
Seed belongs to a homologous doping series (same element set, varying oxygen).
Series = O x Ca1 Cu2 Ba2 Hg1. Found 12 members via search_db("Ca1Cu2Ba2Hg1", pages 1-6):
6.080, 6.100, 6.130, 6.140, 6.160, 6.190, 6.210, 6.220, 6.240, 6.270, 6.300, 6.310

Measured Tc:
- O6.080→92.0 (low)
- O6.100→92.5 (low)
- O6.130→102.0 (high)
- O6.140→105.0 (high)
- O6.160→NO SAMPLE (unmeasurable, predict later)
- O6.190→122.0 (high)
- O6.210→127.0 (high) **PEAK**
- O6.220→127.0 (high) **PEAK (tie)**
- O6.240→123.0 (high)
- O6.270→NO SAMPLE (unmeasurable, predict later)
- O6.300→100.0 (high)
- O6.310→98.0 (high)

**T* = 127.0** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1).
Pick peak = O6.210Ca1Cu2Ba2Hg1 (first occurrence).

Unmeasurable series members to predict: O6.160, O6.270.
Budget after Step A: 16/34 used, 18 left.

## Step B — filter family B (contains Hg and exactly 4 elements)
threshold1 = T* - 32 = 127 - 32 = 95.0
Need: contains Hg AND exactly 4 elements, Tc >= 95. Report ALL such (Q1).

Series members have 5 elements (O,Ca,Cu,Ba,Hg) so EXCLUDED from Step B.

search_db("Hg") → 27 matches total, 2/page → 14 pages (all paged, budget 17→30).
4-element candidates (O,Cu,Ba,Hg — pages 1-7): 14 members:
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  (all Cu1Ba2Hg1)
Pages 8-13: series members (5 elem, excluded).
Page 14: O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl — NOT Step B, but Step C candidate).

O4 measurements:
- O4.047→NO SAMPLE (unmeasurable! Step C material)
- O4.097→95.3 (>=95, qualifies Q1)  [budget 31]
- O4.150→97.0 (>=95, qualifies Q1)  [budget 32]
- O4.220→97.0 (>=95, qualifies Q1)  [budget 33]

Budget: 33/34, ONE left.

## Step C — extrapolate (unmeasurable materials)
Unmeasurable found via search_db so far:
- O6.160Ca1Cu2Ba2Hg1 (series, O6.16, between 6.14=105 and 6.19=122)
- O6.270Ca1Cu2Ba2Hg1 (series, O6.27, between 6.24=123 and 6.30=100)
- O4.047Cu1Ba2Hg1 (O4 family, lowest O, between ? and 4.066=? and ... 4.097=95.3)
- O6Cu1Ba2Hg1Tl1 (5-elem Tl-doped, no neighbors measured)

O6 series dome (O vs Tc), measurable:
  6.080→92, 6.100→92.5, 6.130→102, 6.140→105, [6.160=?], 6.190→122, 6.210→127, 6.220→127, 6.240→123, [6.270=?], 6.300→100, 6.310→98
  O6.160: interpolation 105→122 rising → ~113-118. HIGH (>=90). Predict HIGH.
  O6.270: 123→100 falling; 6.270 slightly below 6.300=100 but above 6.240=123 slope. ~100-105. HIGH. Predict HIGH.

O4 family dome (O vs Tc), measurable: 4.097→95.3, 4.150→97, 4.220→97. 4.047 is lowest-O (underdoped tail).
  Underdoped tail below 4.097: Tc decreasing. At 4.047 (just below 4.097=95.3), Tc ~93-95. Borderline ~93 → LOW (<95) for Step B, but for HIGH/LOW (>=90): likely HIGH (~93>=90).
  Actually 4.047 vs 4.097=95.3: difference 0.05 in O. Slope near 4.097 edge ~ mild. Predict ~93-94. HIGH (>=90) but below 95 threshold1.
  → Predict O4.047: HIGH (for Step D high/low); does NOT qualify Step B Q1 (Tc<95).

O6Cu1Ba2Hg1Tl1: Tl-doped 5-elem. No measured neighbors. The O6 CuBaHg base with Tl. CuBaHgO4 family peaks ~97 at O4.15-4.22. With Tl at O6, uncertain. Tl-1212-type (Hg,Tl)1212 reach ~90-100K. Predict HIGH? Ambiguous. With no measured analog, lean on family: Hg-1207/CuBa2Hg with 6 oxygens and Tl — many Tl/Hg cuprates are high. Predict HIGH (uncertain).

## FINAL measurement (budget 34): O6Cu1Ba2Hg1Tl1 → Tc = 0.0 (sample EXISTS, low/not superconducting)
So O6Cu1Ba2Hg1Tl1 is MEASURABLE (Tc=0), NOT unmeasurable. Predicted/extrapolated value not needed.
It is LOW (0 < 90). Does NOT qualify Step B (5 elements anyway, and 0 < 95).

## Budget exhausted: 34/34.

## Summary of unmeasurable materials (must appear in predictions):
1. O6.160Ca1Cu2Ba2Hg1 — predict HIGH (~113-118, rising edge of O6 dome)
2. O6.270Ca1Cu2Ba2Hg1 — predict HIGH (~100-105, falling edge but still >90)
3. O4.047Cu1Ba2Hg1 — predict HIGH (~93-94, just under 4.097=95.3; >=90 but <95)

O6Cu1Ba2Hg1Tl1 is MEASURABLE (Tc=0.0), so NOT in predictions. It's LOW.

## Rule induced (Step C)
Tc follows an oxygen-doping DOME: rises with O to a peak then falls. For each material,
interpolate within its own homologous family (same element set) using the nearest
measured neighbors; if the interpolated Tc >= 90 K, predict HIGH else LOW.
- O6.160: between O6.140(105) & O6.190(122), rising → ~113 → HIGH
- O6.270: between O6.240(123) & O6.300(100), falling → ~112 → HIGH
- O4.047: just below O4.097(95.3) on underdoped tail → ~93 → HIGH (but <95, so not Q1)
Single-factor rules (e.g., "lowest-O always low") fail: O6.080(92) is high though low-O;
the dome shape, not a single factor, determines it.

## Step D — final pick
Unmeasurable predicted HIGH: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.047Cu1Ba2Hg1.
Fewest elements: O6.160 & O6.270 have 5 elements (O,Ca,Cu,Ba,Hg); O4.047 has 4 (O,Cu,Ba,Hg).
→ Fewest elements = O4.047Cu1Ba2Hg1 (4 elements).
Tie-break not needed (unique fewest).
**FINAL = O4.047Cu1Ba2Hg1**

## Step B Q1 (contains Hg, exactly 4 elements, Tc >= 95):
Candidates measured: O4.097(95.3✓), O4.150(97.0✓), O4.220(97.0✓).
Unmeasured 4-elem: O4.066, O4.081, O4.085, O4.096, O4.101, O4.103, O4.160, O4.162, O4.180, O4.200, O4.047(no sample).
  - O4.047 unmeasurable → not in Q1 (no measured Tc; predicted ~93 < 95).
  - Others unknown. Cannot measure (budget exhausted). Conservative: Q1 = the 3 confirmed.
  NOTE: threshold1=95 is just below peak region (97). Neighbors of 95.3 and 97 likely >=95.
  O4.096 (just below 4.097=95.3): ~95 → borderline. O4.101/4.103 (just above 95.3, toward 97): >=95 likely.
  O4.150-4.220 plateau ~97 → all >=95. O4.066,4.081,4.085 (below 4.097): likely <95 (underdoped tail, like 4.047~93).
  Conservative Q1 (only confirmed measured >=95): O4.097, O4.150, O4.220.
  Given plateau logic, strong candidates also: O4.101, O4.103, O4.160, O4.162, O4.180, O4.200.
  But to be faithful (only report confirmed via measurement), Q1 = [O4.097, O4.150, O4.220].
