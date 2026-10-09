# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K
- Budget: 34 measure_tc calls
- High-level goal: 4 steps (A series peak, B filter, C extrapolate rule, D final pick)

## Composition parsing
Elements = letter-prefixed tokens (O, Ca, Cu, Ba, Hg, Tl, Y, La, Sr, Pb, Cd).
Element count = number of distinct element symbols present.

## Step A — series peak (seed family: O?Ca1Cu2Ba2Hg1)
Seed family in measurable.txt (oxygen-doped series, same element set Ca,Cu,Ba,Hg,O):
- O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310

Plan: measure all 10, find max Tc.

## Step A — results (seed family O?Ca1Cu2Ba2Hg1, 5 elem {O,Ca,Cu,Ba,Hg})
O6.080=92.0, O6.100=92.5, O6.130=102.0, O6.140=105.0, O6.190=122.0,
O6.210=127.0, O6.220=127.0, O6.240=123.0, O6.300=100.0, O6.310=98.0
=> T* = 127.0 (plateau at O6.210/O6.220). peak = O6.210Ca1Cu2Ba2Hg1.

## Step B — threshold1 = 127.0 - 32 = 95.0 K
Hg + exactly-4-elements family in measurable.txt = O4.0xxCu1Ba2Hg1 (elems {O,Cu,Ba,Hg}).
Measured (budget 11-21):
O4.081=98.0, O4.162=97.4, O4.150=97.0, O4.220=97.0, O4.096=95.6,
O4.097=95.3, O4.047=95.0, O4.200=94.2, O4.101=93.4, O4.103=92.5, O4.085=92.0
Q1 (Tc>=95.0) = {O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047}Cu1Ba2Hg1  (7 members)

## Other measurables (budget 22-26)
O6Cu1Ba2Hg1Tl1 = 0.0 (LOW; Hg+Tl mix, no Ca)
O10Ca2Cu3Ba2Tl2 = 118.97 (HIGH; Tl+Ca, Tl-1223)
O6Cu1Cd0.250Ba2Tl1.750 = 88.0 (LOW; Tl no Ca, Cd sub)
O7Cu3Ba2La1 = 89.6 (LOW; no Hg/Tl, La-123)
O7Cu3Y0.200Ba2La0.800 = 81.0 (LOW; no Hg/Tl, Y/La-123)
Total budget used: 26/34. All measurables done. Unmeasurables have no sample.

## Step C — induced rule
Single-factor rules fail: "contains Hg" wrong (O6Cu1Ba2Hg1Tl1 has Hg, Tc=0);
"5 elements" wrong (mixed). Seed-series all-high across O6.080→6.310 (92→127K)
shows rule is qualitative (element presence), not stoichiometric.
RULE: High iff (contains Hg AND not Tl) OR (contains Tl AND contains Ca).
Verified on all 26 measurables (22 high, 4 low) — perfect separation.

## Step C — predictions for unmeasurables
O6.160Ca1Cu2Ba2Hg1  -> Hg,¬Tl -> high   (Hg-1212, ~113K interp)
O6.270Ca1Cu2Ba2Hg1  -> Hg,¬Tl -> high   (Hg-1212, ~118K interp)
O4.180Cu1Ba2Hg1     -> Hg,¬Tl -> high   (Hg-1201, ~97K interp)
O8Ca0.850Cu2.200Ba2Tl1.800 -> Tl,Ca -> high
O7Ca1Cu2Sr2Tl0.500Pb0.500  -> Tl,Ca -> high
O8Cu3Y1Ba1.900La0.100  -> no Hg, no Tl -> low
O8Cu4Y1Ba1.870La0.130  -> no Hg, no Tl -> low

## Step D — final pick (fewest elements among predicted-high; tiebreak higher Tc)
Element counts of predicted-high: #1=5, #2=5, #3=4, #4=5, #5=6.
Fewest = #3 = O4.180Cu1Ba2Hg1 (4 elem, Hg-1201). No tie.
Robust: #3 is pure Hg-1201 (all measured Hg-1201 are high 92-98K); stays high
under any reasonable rule, so final is invariant to rule ambiguity on #4/#5.
FINAL = O4.180Cu1Ba2Hg1
