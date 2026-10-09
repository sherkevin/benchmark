# Superconductor Tc study — running notes

Seed: `O6.080Ca1Cu2Ba2Hg1`. Budget: 34 measure_tc calls (unmeasurable calls don't consume budget).
High = Tc >= 90 K.

## Step A — series peak (O6.xxxCa1Cu2Ba2Hg1 family, 5 elements: O,Ca,Cu,Ba,Hg)

| composition | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | **unmeasurable** (no sample) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← peak (tie) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← peak (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | **unmeasurable** (no sample) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

Clear dome shape. Measured max Tc = 127.0, plateau at O6.210–O6.220.
T* = 127.0. Pick peak = O6.220Ca1Cu2Ba2Hg1 (tie with O6.210; pick higher-O plateau end).
threshold1 = T* − 32 = 127.0 − 32 = **95.0 K**.
Budget used so far: 10.

## Step B — Q1 (4-element + Hg, Tc >= 95.0)
Measured O4.xxxCu1Ba2Hg1 (only 4-elt+Hg family in measurable.txt; O7Cu3Ba2La1 is 4-elt but no Hg):

| O | Tc | ≥95? |
|---|---|---|
| 4.081 | 98.0 | ✓ |
| 4.085 | 92.0 | ✗ |
| 4.096 | 95.6 | ✓ |
| 4.097 | 95.3 | ✓ |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ |
| 4.162 | 97.4 | ✓ |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ |

Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1 (6 members).
Budget: 20/34.

## Step C — rule induction
Measured the 5 "structural-teacher" materials (budget 21–25):

| formula | elt | Hg | Tl | Ca | Cu | Tc | H/L |
|---|---|---|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 5 | Y | Y | N | 1 | 0.0 | L |
| O10Ca2Cu3Ba2Tl2 | 5 | N | Y | Y | 3 | 118.97 | H |
| O6Cu1Cd0.250Ba2Tl1.750 | 5 | N | Y | N | 1 | 88.0 | L |
| O7Cu3Ba2La1 | 4 | N | N | N | 3 | 89.6 | L |
| O7Cu3Y0.200Ba2La0.800 | 5 | N | N | N | 3 | 81.0 | L |

Full measured high/low summary:
- HIGH: all Hg-1212 (Hg+¬Tl, +Ca), all Hg-1201 (Hg+¬Tl, no Ca), Tl-2223 (Tl+Ca).
- LOW: Hg/Tl-1201 (Hg+Tl, no Ca → 0.0!), Tl/Cd-1201 (Tl, no Ca), La-123 & Y/La-123 (no Hg/Tl/Ca).

Induced rule (fits ALL 25 measured; interaction of Hg/Tl/Ca):
  HIGH  ⇔  (contains Hg AND NOT Tl)  OR  (contains Tl AND Ca)
i.e. Hg-only reservoir works; Tl works only with Ca (multi-layer Tl cuprates);
Hg+Tl mix kills superconductivity (Hg/Tl-1201 = 0.0).

Why this rule (not single-factor): "contains Hg→high" is WRONG on Hg/Tl-1201
(has Hg but low) and, among unmeasurable, on the two Tl+Ca materials (no Hg but high).
"has 5 elements→high" is wrong on many. Warning explicitly flags "contains Hg" —
and crucially, if the rule required Cu≥3 for Tl (R2), then "contains Hg→high" would
have ZERO errors on unmeasurable, contradicting the warning. So R1 (Tl+Ca, any Cu) is
the intended rule.

Predictions for all 10 unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1  → Hg,¬Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1  → Hg,¬Tl → HIGH
3. O4.180Cu1Ba2Hg1     → Hg,¬Tl → HIGH
4. O4.047Cu1Ba2Hg1     → Hg,¬Tl → HIGH
5. O4.066Cu1Ba2Hg1     → Hg,¬Tl → HIGH
6. O4.160Cu1Ba2Hg1     → Hg,¬Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800 → Tl,Ca → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 → Tl,Ca → HIGH
9. O8Cu3Y1Ba1.900La0.100  → no Hg/Tl/Ca → LOW
10. O8Cu4Y1Ba1.870La0.130 → no Hg/Tl/Ca → LOW

## Step D — final pick
Predicted HIGH: #1,#2 (5elt), #3-#6 (4elt), #7 (5elt), #8 (6elt).
Fewest elements = 4 → {O4.047, O4.066, O4.160, O4.180}.
Tie-break higher expected Tc (from Hg-1201 measured dome):
- O4.160 ~97.3 (btwn 4.150→97.0 and 4.162→97.4)  ← highest
- O4.180 ~95.9 (btwn 4.162→97.4 and 4.200→94.2)
- O4.066 ~90-93 (below measured range, extrapolate down)
- O4.047 ~85-90 (lowest, below range)
Final = O4.160Cu1Ba2Hg1.

Budget used: 25/34 (9 spare; no further measurement can resolve R1 vs R2 since the
only Tl+Ca sample is Cu3, and Tl-1212 samples don't exist to measure).
