# Superconductor Tc study — notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg — 5 elements)
- Budget: 26 units total (shared between measure_tc and search_db)
- High = Tc >= 90 K, Low = Tc < 90 K

### Steps
- A: Find homologous doping series (same elements, varying O). Measure members. Max Tc = T*.
- B: threshold1 = T* - 32. Among Hg-containing materials with EXACTLY 4 elements, report all with Tc >= threshold1 = Q1.
- C: For unmeasurable materials (no sample), induce rule, predict high/low for each. Single-factor rules fail on several.
- D: Among predicted-high unmeasurable, pick FEWEST elements (tie-break: higher expected Tc).

### Answer JSON keys
peak, peak_tc, threshold, qualifying, predictions (ALL unmeasurable found), final, rule

## Step A — series (O6.xCa1Cu2Ba2Hg1, 12 members) MEASURED
Sorted by O content:
- O6.080 → 92.0
- O6.100 → 92.5
- O6.130 → 102.0
- O6.140 → 105.0
- O6.160 → NULL (unmeasurable)
- O6.190 → 122.0
- O6.210 → 127.0  ← T* (tie max)
- O6.220 → 127.0  ← T* (tie max)
- O6.240 → 123.0
- O6.270 → NULL (unmeasurable)
- O6.300 → 100.0
- O6.310 → 98.0

**T* = 127.0** at O6.210 and O6.220 (tie). Pick O6.220Ca1Cu2Ba2Hg1 as peak (last/max).
Actually tie — pick O6.220 (higher O, last of the plateau). Both valid; I'll use O6.220Ca1Cu2Ba2Hg1.
Hmm, tie at 127. Convention: pick the one... either is correct value. Use O6.220Ca1Cu2Ba2Hg1.

## Step B — threshold1 = T* - 32 = 127 - 32 = 95.0
Hg-containing + exactly 4 elements. From Hg search:
- O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 Cu1Ba2Hg1 (all 4-element: O,Cu,Ba,Hg)
- O6.xCa1Cu2Ba2Hg1 = 5 elements (excluded)
- O6Cu1Ba2Hg1Tl1 = 5 elements (excluded)
Need Tc >= 95.0 for Q1.

## Step C — unmeasurable found
- O6.160Ca1Cu2Ba2Hg1 (series, 5 elem)
- O6.270Ca1Cu2Ba2Hg1 (series, 5 elem)
Possibly more among O4 family if some are unmeasurable. Need to check.

## Step B — O4.xCu1Ba2Hg1 measurements (4-element family, threshold 95)
- O4.103 → 92.5 (< 95, low)
- O4.150 → 97.0 (>= 95, HIGH) ✓ Q1
- O4.180 → NULL (unmeasurable) — predict
- O4.220 → 97.0 (>= 95, HIGH) ✓ Q1

O4 family dome: rises 92.5 → 97, peak ~97 near O4.150-O4.220. By symmetry O4.180 (mid) likely ~97-98 (≥95, predicted high).

## Step C — unmeasurable materials discovered
1. O6.160Ca1Cu2Ba2Hg1 — series dome: between 105 (6.140) and 122 (6.190). Interpolate ~115-120. HIGH (≥90).
2. O6.270Ca1Cu2Ba2Hg1 — series dome: between 123 (6.240) and 100 (6.300). Interpolate ~111. HIGH (≥90).
3. O4.180Cu1Ba2Hg1 — O4 family: between 97 (4.150) and 97 (4.220). ~97. HIGH (≥90).

All 3 predicted HIGH based on interpolation within measured domes.
**Rule**: Within a homologous doping series, Tc varies smoothly (dome). For an unsynthesized member, predict Tc by interpolation between nearest measured neighbors (or plateau extension); classify high/low vs 90 K threshold. Single-factor rules (e.g. "all O6 series high", or "all O4 high", or "O content alone") fail because they ignore the smooth dome shape and local neighbors.

## Step D — final pick
Predicted-high unmeasurables: O6.160 (5 elem), O6.270 (5 elem), O4.180 (4 elem).
Fewest elements → O4.180Cu1Ba2Hg1 (4 elements). Tie-break n/a (only one with 4).
**final = O4.180Cu1Ba2Hg1**

## Final O4 measurements (budget 25, 26)
- O4.047 → 95.0 (≥95, HIGH) ✓ Q1
- O4.200 → 94.2 (<95, LOW, does not qualify)

## Q1 (Hg + exactly 4 elements + Tc ≥ 95.0):
1. O4.047Cu1Ba2Hg1 (95.0)
2. O4.150Cu1Ba2Hg1 (97.0)
3. O4.220Cu1Ba2Hg1 (97.0)
(Not O4.103 [92.5], O4.200 [94.2]. Unmeasured O4 members' Tc inferred below 95 from the dome; only the ≥95 region qualifies, but Q1 reports ALL whose Tc ≥ threshold. Since I couldn't measure all 14, I report the confirmed-qualifying measured ones plus the unmeasurable O4.180 if predicted ≥95.)

## Q1 decision (final)
Report confirmed-qualifying measured members: O4.047, O4.150, O4.220.
O4.180 is unmeasurable (4-element, predicted ~97 ≥95) — but Step B asks to "measure" — it's unmeasurable so cannot confirm. Include in Q1 only via prediction? Step B says "report ALL whose Tc ≥ threshold1". O4.180 predicted ~97 ≥ 95 → include.
Final Q1 = [O4.047Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.180Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]
(ordered by O content)

## Predictions (all unmeasurable found via search_db):
- O6.160Ca1Cu2Ba2Hg1 → high (interp 115-120)
- O6.270Ca1Cu2Ba2Hg1 → high (interp ~111)
- O4.180Cu1Ba2Hg1 → high (interp ~97)

## Budget mechanic confirmed
- Null/unmeasurable measurements are FREE (don't consume budget). Verified: 1st batch 6 calls only charged 4 (2 nulls free). This is the designed way to find Step C unmeasurables.
- Budget exhausted at 26/26 after O4.047 (25) and O4.200 (26).
- O4.066, O4.160 returned "unknown composition; discover via search_db first" = measurable but not in discovered set (quirk), NOT unmeasurable. Exclude from Step C.
- O4.081/085/096/097/101/162 returned "instrument time exhausted" = measurable (would charge), not null. So no hidden unmeasurables there.

## Final O4 family data (4-elem Hg, threshold 95)
- O4.047 → 95.0 ✓Q1
- O4.103 → 92.5 ✗
- O4.150 → 97.0 ✓Q1
- O4.160 → (unmeasured, measurable; "unknown comp" err) ~97 predicted
- O4.162 → (unmeasured, measurable) ~97 predicted
- O4.180 → NULL unmeasurable, predicted ~97 ✓Q1 (≥95)
- O4.200 → 94.2 ✗
- O4.220 → 97.0 ✓Q1
(others O4.066/081/085/096/097/101 unmeasured-measurable, predicted <95 by interp, not in Q1)

## FINAL ANSWER
- peak: O6.220Ca1Cu2Ba2Hg1 (Tc 127, tie with O6.210)
- peak_tc: 127.0
- threshold: 95.0
- qualifying (Q1, 4-elem Hg + Tc≥95): [O4.047, O4.150, O4.180, O4.220] Cu1Ba2Hg1
- predictions (all confirmed unmeasurable): {O6.160Ca1Cu2Ba2Hg1: high, O6.270Ca1Cu2Ba2Hg1: high, O4.180Cu1Ba2Hg1: high}
- final: O4.180Cu1Ba2Hg1 (4 elements, fewest among predicted-high unmeasurables)
- rule: smooth-dome interpolation within homologous doping series

## Budget used: 26/26 (exhausted). DONE.
