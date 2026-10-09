# Superconductor Tc study — notes

Budget: 18 units total (shared measure_tc + search_db).

Seed: `O6.080Ca1Cu2Ba2Hg1` — Hg-1212 family, elements {O,Ca,Cu,Ba,Hg} = 5 el.
Definition: high = Tc >= 90 K.

## Full Hg inventory (search "Hg" pages 1-9, 3/page, 27 total) — cost 9
4-element family `O4.xxxCu1Ba2Hg1` (Hg-1201, {O,Cu,Ba,Hg}) — 14 members:
  4.047, 4.066, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.160, 4.162, 4.180, 4.200, 4.220
5-element seed series `O6.xxxCa1Cu2Ba2Hg1` (Hg-1212, {O,Ca,Cu,Ba,Hg}) — 12 members:
  080(seed), 100, 130, 140, 160, 190, 210, 220, 240, 270, 300, 310
5-element `O6Cu1Ba2Hg1Tl1` ({O,Cu,Ba,Hg,Tl}) — 1.

## Budget tracking
Search Hg 9 pages: 9 used. Remaining: 9.

## ALL measurements log (budget 18 total)
O6.080Ca1Cu2Ba2Hg1 = 92.0   ✓meas
O6.100Ca1Cu2Ba2Hg1 = 92.5   ✓meas
O6.160Ca1Cu2Ba2Hg1 = UNMEASURABLE
O6.190Ca1Cu2Ba2Hg1 = 122.0  ✓meas
O6.210Ca1Cu2Ba2Hg1 = 127.0  ✓meas (peak tie)
O6.220Ca1Cu2Ba2Hg1 = 127.0  ✓meas (peak tie)
O6.270Ca1Cu2Ba2Hg1 = UNMEASURABLE
O6.310Ca1Cu2Ba2Hg1 = 98.0   ✓meas
O4.162Cu1Ba2Hg1 = 97.4  ✓meas (>=95, Q1)
O4.180Cu1Ba2Hg1 = UNMEASURABLE (predicted ~95-97 → high; Step D pick)
O4.200Cu1Ba2Hg1 = 94.2  ✓meas (<95, not Q1)
O4.220Cu1Ba2Hg1 = 97.0  ✓meas (>=95, Q1)
Measurements used: 9. Total budget used: 18 (exhausted).

## O4 dome near peak
4.162=97.4 (peak), 4.180=unmeas, 4.200=94.2, 4.220=97.0.
O4.180 interp (linear 4.162→4.200) ≈ 95.9; (parabola) ≈ 96.7. Both >=95 and >=90. → predicted high.

## Element-count check (for "exactly 4 elements" / "fewest elements")
O4.xxxCu1Ba2Hg1 = {O,Cu,Ba,Hg} = 4 elements.
O6.xxxCa1Cu2Ba2Hg1 = {O,Ca,Cu,Ba,Hg} = 5 elements.
O6Cu1Ba2Hg1Tl1 = {O,Cu,Ba,Hg,Tl} = 5 elements.
→ 4-element Hg materials = the O4 family only.

## FINAL ANSWERS
peak = O6.220Ca1Cu2Ba2Hg1 (Tc 127, ties O6.210 at the plateau max)
peak_tc (T*) = 127
threshold1 = 127 - 32 = 95
Q1 (4-element Hg, measured Tc >= 95) = [O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0)]
   (O4.200=94.2 excluded; O4.180 unmeasurable → handled in Step C/D, not measured-Tc Q1)
predictions (all confirmed-unmeasurable found) =
   O6.160Ca1Cu2Ba2Hg1 → high (interp ~112)
   O6.270Ca1Cu2Ba2Hg1 → high (interp ~111)
   O4.180Cu1Ba2Hg1 → high (interp ~96)
final (Step D: predicted-high unmeasurable, fewest elements) = O4.180Cu1Ba2Hg1 (4 el < 5 el)
rule = oxygen-doping-dome interpolation within each homologous family (multi-factor).

## Step A — T*
Dome shape: 080(92)→100(92.5)→[160 unmeas]→190(122)→210(127)→220(127)→[270 unmeas]→310(98).
Plateau at 210-220 = 127 K; descending on both sides. T* = 127.
Peak composition: O6.220Ca1Cu2Ba2Hg1 (ties O6.210).

## Step B
threshold1 = T* - 32 = 127 - 32 = 95 K.
Q1 = materials with Hg AND exactly 4 elements AND Tc >= 95.
4-element Hg family = O4.xxxCu1Ba2Hg1.
Measured O4 dome: 4.162=97.4, 4.180=UNMEAS, 4.200=94.2, 4.220=97.0.
Q1 (measured, qualifying): O4.162Cu1Ba2Hg1 (97.4), O4.220Cu1Ba2Hg1 (97.0).
O4.200 (94.2) does NOT qualify (below 95).
O4.180 unmeasurable — predicted? See Step C.

## Step C — unmeasurable materials discovered via search_db
Found (only via search_db, since I couldn't measure all):
  O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1
Possibly others (O6.130, 140, 240, 300; O4.047, 066, 081, 085, 096, 097, 101, 103, 150, 160; O6Cu1Ba2Hg1Tl1) — I did NOT measure these, so I do NOT know if they are unmeasurable. Task says predict for "every unmeasurable one you find". I only KNOW these 3 are unmeasurable (returned "no sample"). I'll report predictions only for these 3 (the unmeasurable materials I found).

### Rule induction
"Warning: single-factor rules are wrong on several of them."
Single factor = just O-value? Both series are doping domes in O. A rule based only on O-content (e.g. "O6 family high, O4 family low") would be a single-factor rule.
The data shows it's NOT just element-family:
  - O6.080 = 92 (high), O6.100 = 92.5 (high), O6.190 = 122 (high), O6.310 = 98 (high) → all measured O6 are high.
  - O4.200 = 94.2 (high, but below threshold), O4.162 = 97.4 (high), O4.220 = 97 (high) → measured O4 near peak are high; O4 below 4.15 likely low.
The CORRECT multi-factor rule = position relative to the doping dome peak (parabolic in O), combined with element-count layer:
  - Predict Tc by interpolating along the measured O-dome of the same homologous family; high if interpolated Tc >= 90, else low.
O6.160: between O6.100(92.5) and O6.190(122), rising side → interp ~107-115 → HIGH.
O6.270: between O6.220(127) and O6.310(98), falling side → interp ~112-118 → HIGH.
O4.180: between O4.162(97.4) and O4.200(94.2)/4.220(97), near peak → interp ~95-97 → HIGH (>=90, and >=95 threshold too).

A pure single-factor rule would fail: e.g. "O6.x with x near 6.1 low" is false (190 is high). And O6.080/100 are high despite low O. Position-in-dome (multi-factor: family + O-distance-from-peak + side) is the robust rule.

## Step D — final pick
Among unmeasurable materials predicted HIGH, choose FEWEST elements (tie → higher expected Tc).
Unmeasurable + predicted high: O6.160Ca1Cu2Ba2Hg1 (5 el), O6.270Ca1Cu2Ba2Hg1 (5 el), O4.180Cu1Ba2Hg1 (4 el).
Fewest elements = O4.180Cu1Ba2Hg1 (4 elements).
FINAL = O4.180Cu1Ba2Hg1.

(Its expected Tc ~95-97 from O4 dome interpolation near the peak; only one with 4 elements among the predicted-high unmeasurables.)
