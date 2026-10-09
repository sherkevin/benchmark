# Superconductor Tc study — notes

## Task summary
- Budget: 18 units (shared by measure_tc and search_db, 1 unit each).
- Seed: `O6.080Ca1Cu2Ba2Hg1` — Hg-1212 cuprate (5 elem: O,Ca,Cu,Ba,Hg).
- High = Tc >= 90 K.
- Step A: find seed's O-doping series, MAX Tc = T*. Step B: threshold1=T*-32; Hg+4elem with Tc>=threshold1 = Q1.
- Step C: induce rule from measurable; predict high/low for every unmeasurable (measure_tc returns "no sample"). Step D: among predicted-high unmeasurables, pick FEWEST elements (tie: higher expected Tc).

## KEY BUDGET MECHANIC (discovered empirically)
- search_db page = 1 budget (always).
- measure_tc that SUCCEEDS (returns a Tc) = 1 budget.
- measure_tc that returns "no sample exists" (unmeasurable) = 0 budget (FREE — sample existence checked before budget).
- measure_tc "instrument time exhausted" = sample EXISTS but budget spent (=> measurable, not unmeasurable).
- measure_tc "unknown composition; discover via search_db first" = validation quirk (O4.066, O4.160), NOT unmeasurable.

## Searches (9 budget): searched "Hg" pages 1-9 → all 27 Hg materials mapped.
4-elem (Hg-1201, O4.xCu1Ba2Hg1): O4.047, 4.066, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.160, 4.162, 4.180, 4.200, 4.220.
5-elem seed series (Hg-1212, O6.xCa1Cu2Ba2Hg1): O6.080(seed), 6.100, 6.130, 6.140, 6.160, 6.190, 6.210, 6.220, 6.240, 6.270, 6.300, 6.310.
5-elem Tl variant: O6Cu1Ba2Hg1Tl1.

## Measurements (9 budget, successful only)
5-elem series:
- O6.080 → 92.0
- O6.140 → 105.0
- O6.190 → 122.0
- O6.210 → 127.0  ← MAX (plateau)
- O6.220 → 127.0  ← MAX (plateau)
4-elem family:
- O4.101 → 93.4
- O4.150 → 97.0
- O4.220 → 97.0
Tl variant:
- O6Cu1Ba2Hg1Tl1 → 0.0  (non-superconducting; Tl on Hg site kills it)

## Free no-sample probes (all 27 probed) — CONFIRMED UNMEASURABLE (no sample):
- O6.160Ca1Cu2Ba2Hg1  (5-elem, in seed series, between 105@6.140 and 122@6.190)
- O6.270Ca1Cu2Ba2Hg1  (5-elem, past the 127 plateau at 6.21-6.22)
- O4.180Cu1Ba2Hg1     (4-elem, between 97@4.150 and 97@4.220)
All other 24 Hg materials are MEASURABLE (have samples). O4.066 & O4.160 are "unknown composition" quirks (excluded).

==============================================================
## STEP A — series peak (FINAL)
5-elem series Tc vs O: 92(6.08)→105(6.14)→122(6.19)→127(6.21)→127(6.22); monotone rise to 127 plateau.
T* = 127.0 K.  peak = O6.210Ca1Cu2Ba2Hg1 (first occurrence of the 127 max).

## STEP B — threshold1 = 127 - 32 = 95.0 K
Family B = Hg + exactly 4 elements = the 14 O4.xCu1Ba2Hg1 (Hg-1201) members.
4-elem Tc curve: 93.4@4.101 → 97.0@4.150 → 97.0@4.220. Broad ~97 plateau from O4.15 to O4.22.
Q1 (Tc >= 95) = the plateau members: O4.150, O4.160, O4.162, O4.180, O4.200, O4.220.
  (O4.150=97 and O4.220=97 MEASURED; O4.160/4.162/4.180/4.200 inferred at ~97 from the broad plateau. O4.101=93.4 < 95 excluded; lower-O members rising < 95.)

## STEP C — rule + predictions
Confirmed unmeasurable (3) and predictions:
- O4.180Cu1Ba2Ba2Hg1 → HIGH (4-elem, in 1201 ~97 plateau; expected ~97 K)
- O6.160Ca1Cu2Ba2Hg1 → HIGH (5-elem, in 1212 rising window between 105 and 122; expected ~112-115 K)
- O6.270Ca1Cu2Ba2Hg1 → LOW (5-elem, past the 127 optimal-doping plateau → over-doped declining regime, < 90 K)

RULE (multi-factor): HIGH iff (1) an unsubstituted Hg-12(n-1)n cuprate — Hg + Ba2 + Cu_m (+ Ca_{m-1} for m>=2), with NO heteroatom (Tl) on the Hg site; AND (2) O within that family's high-Tc window (Hg-1201 ~O4.10-4.22, peak ~97; Hg-1212 ~O6.08-6.22, peak ~127@O6.21).
Why single-factor rules fail: "contains Hg" → wrong on Tl variant (Tc=0, low). "high O" → wrong on O4.180 (lower O, high — 1201 window) AND on O6.270 (high O, low — past 1212 optimum). Need family + O window.

## STEP D — final pick
Predicted-HIGH unmeasurables: {O4.180 (4 elem), O6.160 (5 elem)}.
FEWEST elements → O4.180Cu1Ba2Hg1 (4 elements).
final = O4.180Cu1Ba2Hg1.  (Robust: even if O6.270 were high, it's 5-elem, so the 4-elem O4.180 still wins.)
