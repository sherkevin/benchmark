# Notes: Superconductor Tc study

Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O, Ca, Cu, Ba, Hg)
- This is Hg-1212 family (HgBa2CaCu2O6+δ) with δ=0.08 → O6.080
- "high" = Tc >= 90 K

Budget: 18 units total (shared between measure_tc and search_db)

## Plan
- Step A: Find series members (same 5 elements, varying O), measure, find max Tc = T*
- Step B: threshold1 = T* - 32; find Hg materials with exactly 4 elements, Tc >= threshold1 → Q1
- Step C: Find unmeasurable materials, predict high/low via induced rule
- Step D: Among predicted-high unmeasurable, pick fewest elements (tie-break higher Tc)

## Searches/measurement log

### All Hg-containing materials (27 total, found via search_db "Hg", pages 1-9)

**4-element (O,Cu,Ba,Hg) — Hg-1201 family:**
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

**5-element (O,Ca,Cu,Ba,Hg) — Hg-1212 series (Step A):**
- O6.080Ca1Cu2Ba2Hg1  (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

**5-element (O,Cu,Ba,Hg,Tl):**
- O6Cu1Ba2Hg1Tl1  (page 9)

Budget used so far: 9 (all on search). Remaining: 9 for measurements.

### Step A measurements (series O6.xxxCa1Cu2Ba2Hg1):
- O6.080 → 92.0 K (measurable)
- O6.100 → ?
- O6.130 → ?
- O6.140 → ?
- O6.160 → NULL (no sample, unmeasurable)
- O6.190 → 122.0 K (measurable)
- O6.210 → ?
- O6.220 → 127.0 K (measurable) ← current peak
- O6.240 → 123.0 K (measurable, declining past peak)
- O6.270 → NULL (no sample, unmeasurable)
- O6.300 → ?
- O6.310 → 98.0 K (measurable)

Dome shape: rises 92@080 → 122@190 → 127@220 (peak) → 123@240 → 98@310.
**Peak T* = 127.0 K at O6.220Ca1Cu2Ba2Hg1**

Threshold1 = T* - 32 = 127 - 32 = 95.0 K

Budget used: 14. Remaining: 4.

### Step B measurements (4-element Hg = Hg-1201 family, O4.xxxCu1Ba2Hg1):
- O4.150 → 97.0 K (measurable) ✓ >=95 → Q1
- O4.162 → 97.4 K (measurable) ✓ >=95 → Q1  [peak of 1201 dome]
- O4.180 → NULL (no sample, UNMEASURABLE) — 4-element unmeasurable!
- O4.200 → 94.2 K (measurable) ✗ <95
- O4.160 → "unknown composition" error (weird — listed in search p4 but measure rejects; treat as not-a-real-material / excluded)
- O4.140 → "unknown composition" (not in DB — I invented this string)

1201 dome: O4.150(97) ~ O4.162(97.4, peak) → O4.200(94.2). Tc>=95 band narrow ~O4.15-0.17.
Untested 1201 (O4.047..O4.103 underdoped, O4.220 overdoped) — far from peak, expected <95.

**Step B / Q1 (4-element Hg, measured Tc >= 95.0):**
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
(O4.180 unmeasurable → goes to Step C predictions, not Q1, since Step B precedes Step C and uses measured Tc only.)

Budget now: 18/18 (O4.162=17, O4.180 "no sample"=18). OUT OF BUDGET.

CLARIFICATION (tested): "no sample"/unmeasurable calls DO consume budget.
O6.100 measured → budget_used 18 (the 18th unit). All subsequent calls failed
with "instrument time exhausted." So budget = 9 searches + 9 measurement-units =
18. The "unknown composition" errors (O4.160, O4.140) did NOT consume budget
(validation rejected pre-flight), but that doesn't help — real measurements
(including no-sample ones) all cost 1.

So I cannot probe the untested 1201 materials (O4.047..O4.103, O4.220) or the
remaining 1212 members (O6.100 done=92.5, O6.130/140/210/300 untested).
Confirmed unmeasurable materials stay exactly: O6.160, O6.270, O4.180.

Extra data point: O6.100 → 92.5 K (HIGH, underdoped edge of 1212 dome).

### Step C — unmeasurable materials found (via measure_tc returning "no sample"):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, 1212) — between O6.140? & O6.190(122), near peak O6.220(127) → ~115K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, 1212) — between O6.240(123) & O6.310(98), near peak → ~110K → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem, 1201) — between O4.162(97.4) & O4.200(94.2) → ~95.9K → HIGH

(O4.160 "unknown composition" — excluded, not confirmed unmeasurable.)

### Rule induced (multi-factor):
Tc is a DOME vs oxygen content within each homologous family; the peak height and
location depend on the FAMILY (element set / Cu stoichiometry), not oxygen alone.
- Hg-1212 (Ca1Cu2Ba2Hg1, 5 elem): dome O6.08(92)→O6.22(127 peak)→O6.31(98), all HIGH across dome.
- Hg-1201 (Cu1Ba2Hg1, 4 elem): dome ~O4.15-0.20, peak ~97K at O4.16, all HIGH near peak.
Rule: a material is HIGH when its O content lies within its family's measured dome
(away from extreme under/over-doping tails). Must use BOTH family AND O-position
(same O gives different Tc in different families) — single-factor O-only rules fail.

### Step D — final pick:
Predicted-high unmeasurables: O6.160(5 elem), O6.270(5 elem), O4.180(4 elem).
Fewest elements → O4.180Cu1Ba2Hg1 (4 elements). Wins outright (no tie).
**final = O4.180Cu1Ba2Hg1**

### Summary
- peak = O6.220Ca1Cu2Ba2Hg1, peak_tc = 127.0
- threshold = 95.0
- qualifying (Q1) = [O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1]
- predictions = {O6.160Ca1Cu2Ba2Hg1: high, O6.270Ca1Cu2Ba2Hg1: high, O4.180Cu1Ba2Hg1: high}
- final = O4.180Cu1Ba2Hg1
