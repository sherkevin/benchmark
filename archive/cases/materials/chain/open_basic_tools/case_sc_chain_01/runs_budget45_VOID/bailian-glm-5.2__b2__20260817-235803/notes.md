# Superconductor Tc Study — Notes

## Task overview
- Budget: 45 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K, Low = Tc < 90 K
- search_db returned 2 results/page (1 budget/page); measure_tc 1 budget/measurement
- Steps: A (series peak T*), B (threshold=T*-32, Q1 = Hg & exactly-4-element
  materials with Tc>=threshold), C (predict unmeasurable materials high/low),
  D (fewest-element predicted-high unmeasurable; tie-break higher expected Tc).

## Budget tracking
Start 45. Final used 37 (searches 26 + measurements 11). 8 remaining.

## Searches
- "Hg" p1-2: O4.047, O4.066, O4.081, O4.085 Cu1Ba2Hg1 (27 total = 15 1201 + 12 1212)
- "Ca1Cu2Ba2Hg1" (1212 series) 6 pages, 12 members:
  O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
- "Cu1Ba2Hg1" (1201 family) 8 pages, 15 members (14 pure 4-element + 1 Tl 5-element):
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160,
  O4.162, O4.180, O4.200, O4.220 Cu1Ba2Hg1  +  O6Cu1Ba2Hg1Tl1 (5-element, excluded from B)

## Step A — 1212 series Tc vs O (5-element: O,Ca,Cu,Ba,Hg)
| O | Tc |
|---|----|
|6.080|92.0 (seed)|
|6.100|92.5|
|6.130|102.0|
|6.140|105.0|
|6.160|UNMEASURABLE|
|6.190|122.0|
|6.210|127.0 (peak)|
|6.220|127.0 (peak)|
|6.240|123.0|
|6.270|UNMEASURABLE|
|6.300|100.0|
|6.310|98.0|

T* = 127.0 (at O6.210 and O6.220). Peak composition = O6.220Ca1Cu2Ba2Hg1.

## Step B — threshold1 = 127.0 - 32 = 95.0
4-element Hg family = 1201 (O,Cu,Ba,Hg). Measured 1201 Tc:
| O | Tc | Q1 (>=95.0)? |
|---|----|----|
|4.047|UNMEASURABLE|— (predict, Step C)|
|4.066|UNMEASURABLE|— (predict, Step C)|
|4.081|98.0|YES|
|4.085|92.0|no|
|4.096|95.6|YES|
|4.097|95.3|YES|
|4.101|93.4|no|
|4.103|92.5|no|
|4.150|97.0|YES|
|4.160|UNMEASURABLE|— (predict, Step C)|
|4.162|97.4|YES|
|4.180|UNMEASURABLE|— (predict, Step C)|
|4.200|94.2|no|
|4.220|97.0|YES|

Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1.
(O6Cu1Ba2Hg1Tl1 measured Tc=0.0, 5-element, excluded from B entirely.)

## Step C — Rule induction
Across both families, Tc is a CONCAVE (downward) PARABOLA in oxygen stoichiometry
O: low at the low-O end, rises to a peak near optimal O, falls again at the
high-O end. Every measurable point in both families is "high" (>=90), spanning a
broad high band around each family's optimum.

A single-factor monotonic rule (e.g. "higher O → higher Tc", or "high iff O above
a one-sided cutoff") is WRONG on several points because of the parabola's rise AND
fall — that's the task's warning.

Induced rule (two-sided band): For a given homologous Hg-cuprate family, a material
is HIGH iff its oxygen content lies BETWEEN the two O-values where the family's
parabola crosses 90 K (the high band straddling optimal doping); LOW outside it.
Decision uses O position relative to BOTH crossing points, not one factor.

Application:
- Unmeasurable points INSIDE the measured high range → interpolate within band → HIGH.
- Unmeasurable points OUTSIDE the measured range → extrapolate; HIGH only if still
  above the 90 K crossing, else LOW.

### 1201 predictions
Measured range [4.081, 4.220], all high.
- O4.160 — inside [4.150(97.0),4.162(97.4)] → HIGH (~97.2)
- O4.180 — inside [4.162(97.4),4.200(94.2)] → HIGH (~95.8)
- O4.047 — outside low side; below 4.081(98)/4.085(92), parabola descends below 90
  crossing → LOW
- O4.066 — outside low side, below crossing → LOW

### 1212 predictions (5-element series, same parabolic rule; required because they
### were discovered via search_db and are unmeasurable)
Measured range [6.080,6.310], all high.
- O6.160 — inside [6.140(105),6.190(122)] → HIGH (~113)
- O6.270 — inside [6.240(123),6.300(100)] → HIGH (~112)

## Final predictions (ALL unmeasurable discovered via search_db)
{
 "O4.047Cu1Ba2Hg1":"low",
 "O4.066Cu1Ba2Hg1":"low",
 "O4.160Cu1Ba2Hg1":"high",
 "O4.180Cu1Ba2Hg1":"high",
 "O6.160Ca1Cu2Ba2Hg1":"high",
 "O6.270Ca1Cu2Ba2Hg1":"high"
}

## Step D — final pick
Predicted-HIGH unmeasurable materials and element counts:
- O4.160Cu1Ba2Hg1 — 4 elements
- O4.180Cu1Ba2Hg1 — 4 elements
- O6.160Ca1Cu2Ba2Hg1 — 5 elements
- O6.270Ca1Cu2Ba2Hg1 — 5 elements
Fewest elements = 4 → {O4.160, O4.180}. Tie-break higher expected Tc:
- O4.160 ≈ (97.0+97.4)/2 = 97.2
- O4.180 ≈ (97.4+94.2)/2 = 95.8
=> FINAL = "O4.160Cu1Ba2Hg1"
