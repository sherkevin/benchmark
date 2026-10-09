# Superconductor Tc Study — Notes

## Task summary
- Budget: 18 units total (shared between search_db and measure_tc).
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O, Ca, Cu, Ba, Hg).
- high = Tc >= 90 K; low = Tc < 90 K.
- Step A: series peak T* (series = same elements, varying O).
- Step B: threshold1 = T* - 32; Q1 = {materials with Hg AND exactly 4 elements AND Tc >= threshold1}.
- Step C: induce rule from measurable; predict high/low for unmeasurable (measure_tc returns "no sample exists").
- Step D: among unmeasurable predicted high, pick fewest elements (tie-break higher expected Tc).

## Searches & budget log
- search "Hg" p1 -> [O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1]; has_more=true; 27 matches total (3/page). USED=1.
- search "Ca1Cu2Ba2" p1 -> [O6.080, O6.100, O6.130]seed; has_more; 12 matches. USED=2.
- search "Ca1Cu2Ba2" p2 -> [O6.140, O6.160, O6.190]. USED=3.
- search "Ca1Cu2Ba2" p3 -> [O6.210, O6.220, O6.240]. USED=4.
- search "Ca1Cu2Ba2" p4 -> [O6.270, O6.300, O6.310]; has_more=false. USED=5.
  => Seed series (12 members): O6.080, 6.100, 6.130, 6.140, 6.160, 6.190, 6.210, 6.220, 6.240, 6.270, 6.300, 6.310.
- search "Cu1Ba2Hg1" p1 -> [O4.047, O4.066, O4.081]; 15 matches (5 pages). USED=6 (then measurements shift counter).
  NOTE: "Cu1Ba2Hg1" matches Hg-1201 family (O4.xCu1Ba2Hg1) = exactly 4 elements (O,Cu,Ba,Hg). Seed has Cu2+Ca so not matched.
- search "Cu1Ba2Hg1" p2 -> [O4.085, O4.096, O4.097]; has_more=true. USED here later in tracker.
- search "Cu1Ba2Hg1" p3,p4,p5 NOT fetched (out of budget). 9 Hg-1201 members undiscovered.

## Step A measurements (seed series O_x Ca1 Cu2 Ba2 Hg1)
| O      | Tc (K) | status |
|--------|--------|--------|
| 6.080  | 92.0   | measurable |
| 6.140  | 105.0  | measurable |
| 6.190  | 122.0  | measurable |
| 6.210  | 127.0  | measurable  <- PEAK (tied) |
| 6.220  | 127.0  | measurable  <- PEAK (tied) |
| 6.240  | 123.0  | measurable |
| 6.270  | null   | UNMEASURABLE (no sample) |
| 6.300  | 100.0  | measurable |
Not measured (no budget): 6.100, 6.130, 6.160, 6.310.

=> Curve is unimodal with peak ~127 K at O=6.21-6.22, declining to 100 K at O=6.300. T* = 127.0 K.
   Peak composition chosen: `O6.220Ca1Cu2Ba2Hg1` (tied 127.0 with O6.210).
   6.270 is on declining side (between 123 @6.240 and 100 @6.300) => predicted HIGH (~111 K) but below T*.

## Step B — threshold1 = 127.0 - 32 = 95.0 K
4-element Hg materials = Hg-1201 family O4.xCu1Ba2Hg1 (O,Cu,Ba,Hg). Measurements:
| O      | Tc (K) | >= 95? |
|--------|--------|--------|
| 4.047  | 95.0   | YES |
| 4.066  | ERROR "unknown composition; discover via search_db first" (not "no sample"; not unmeasurable by task definition; treated as not-confirmable, excluded) |
| 4.081  | 98.0   | YES |
| 4.085  | 92.0   | no |
| 4.097  | 95.3   | YES |
Not measured: 4.096 (interpolated ~95.2, unconfirmed).

=> Confirmed Q1 (measured, >= 95.0): `O4.047Cu1Ba2Hg1`, `O4.081Cu1Ba2Hg1`, `O4.097Cu1Ba2Hg1`.
   LIMITATION: 9 Hg-1201 members (pages 3-5) + 4.066 + 4.096 not enumerated/measured; some could also qualify. Budget exhausted; cannot confirm.

## Step C — rule & predictions
Confirmed unmeasurable (measure_tc returned "No sample... has not been synthesized", tc=null): only `O6.270Ca1Cu2Ba2Hg1`.
(O4.066 returned a different "unknown composition" error — NOT the task's "no sample exists" signal, so not classified unmeasurable.)

Induced rule (multi-factor; single-factor rules fail on several cases):
  high Tc (>=90 K) requires BOTH:
   (1) Belonging to the Hg-Ba-Cu(-Ca) cuprate family (the high-Tc family here), AND
   (2) Oxygen content within that family's optimal doping window:
       - Hg-1201 (Ba2Cu1Hg1O_x, 4 elem): high for x in ~[4.04, 4.10], peak ~4.08 (~98 K); outside => low.
       - Hg-1212/1223-type (Ba2Ca1Cu2Hg1O_x, 5 elem, seed family): high for x in ~[6.05, 6.30+], peak ~6.21-6.22 (~127 K); outside => low.
  Why single-factor fails: element-count alone can't separate (4-elem O4.085=92 barely high; 5-elem edges low); oxygen alone can't separate (4.085@4.085=92 vs 4.097@4.097=95.3 non-monotonic; 6.080@5-elem=92 vs 4.047@4-elem=95 — O value alone doesn't map without family).

Prediction: O6.270Ca1Cu2Ba2Hg1 -> family = seed (5 elem), O=6.270 within [6.05,6.30] (between 6.240=123 and 6.300=100, both high) => HIGH. Expected Tc ~111 K (linear interp 123->100).

## Step D — final
Only one unmeasurable predicted high: `O6.270Ca1Cu2Ba2Hg1` (5 elements).
=> final = `O6.270Ca1Cu2Ba2Hg1`. (No 4-element unmeasurable confirmed; all measured Hg-1201 members had samples.)

## Budget final: 18/18 exhausted.
