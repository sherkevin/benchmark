# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg — 5 elements)
- High = Tc >= 90 K; Low = Tc < 90 K
- Budget: 24 measure_tc calls

## Series (Step A) — same element set as seed {O,Ca,Cu,Ba,Hg}, varying O
Members in measurable.txt:
- O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310  (all Ca1Cu2Ba2Hg1)
=> 10 measurements for Step A.

## Step B — contains Hg AND exactly 4 elements
4-element Hg materials = the O4.xxxCu1Ba2Hg1 series (elements O, Cu, Ba, Hg):
- O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047, O4.200, O4.101, O4.103, O4.085 (all Cu1Ba2Hg1)
=> 11 measurements for Step B.

## Budget so far: 10 (A) + 11 (B) = 21. Remaining: 3 for Step C rule induction.

## Other (unmeasured) measurable materials — candidates for Step C rule-building
- O6Cu1Ba2Hg1Tl1   (5 elem: O,Cu,Ba,Hg,Tl)  Cu1 → Hg/Tl-1201-like
- O10Ca2Cu3Ba2Tl2  (5 elem: O,Ca,Cu,Ba,Tl)  Cu3 → Tl-2223-like (expect high)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elem: O,Cu,Cd,Ba,Tl) Cu1 → Tl-1201-like w/ Cd
- O7Cu3Ba2La1      (4 elem: O,Cu,Ba,La) Cu3, no Hg/Tl → YBCO-123 analog w/ La
- O7Cu3Y0.200Ba2La0.800 (5 elem: O,Cu,Y,Ba,La) Cu3 → YBCO-like

## Unmeasurable (Step C/D) — must predict, cannot measure
1. O6.160Ca1Cu2Ba2Hg1   (Hg-1212 series, between 6.140 and 6.190)
2. O6.270Ca1Cu2Ba2Hg1   (Hg-1212 series, between 6.240 and 6.300)
3. O4.180Cu1Ba2Hg1      (Hg-1201 series, between 4.162 and 4.200)
4. O8Ca0.850Cu2.200Ba2Tl1.800  (Tl, Cu2.2, Ca → Tl-1212-like)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500   (Tl+Pb, Cu2, Sr → Tl/Pb-1212-like)
6. O8Cu3Y1Ba1.900La0.100       (Cu3, Y → YBCO-123-like)
7. O8Cu4Y1Ba1.870La0.130       (Cu4, Y → Y-124/247-like)

## MEASURED DATA (24/24 used)

### Step A — Hg-1212 series (Ca1Cu2Ba2Hg1, vary O), all 5-element
| O | Tc |   | O | Tc |
|---|----|---|---|----|
| 6.080 | 92.0 | | 6.220 | 127.0 ← MAX |
| 6.100 | 92.5 | | 6.240 | 123.0 |
| 6.130 | 102.0 | | 6.300 | 100.0 |
| 6.140 | 105.0 | | 6.310 | 98.0 |
| 6.190 | 122.0 | | | |
| 6.210 | 127.0 ← MAX | | | |

**T\* = 127.0** (tie O6.210 / O6.220 — pick O6.210, first in file). Bell curve, peak ~O6.21–6.22.
threshold1 = 127 − 32 = **95.0**.

### Step B — Hg-1201 series (Cu1Ba2Hg1, all 4-element {O,Cu,Ba,Hg}), vary O
| O | Tc | ≥95? |
|---|----|------|
| 4.047 | 95.0 | ✓ |
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

**Q1 (Tc≥95.0):** O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220 (7 members).

### Step C calibration (3 "other" materials)
| material | elements | Cu planes | Hg/Tl? | Ca? | Tc | class |
|---|---|---|---|---|---|---|
| O10Ca2Cu3Ba2Tl2 | 5 | 3 | Tl | yes | 118.97 | HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | 5 | 1 | Tl | no | 88.0 | LOW |
| O7Cu3Y0.200Ba2La0.800 | 5 | 3 | none | no | 81.0 | LOW |

## INDUCED RULE (multi-factor interaction)
HIGH (Tc≥90) iff: **(Hg present) OR (Tl present AND ≥2 CuO₂ planes [Cu≥2] AND Ca spacer present).**
Equivalently: a heavy cation (Hg/Tl) charge reservoir must be present; Hg is effective enough to
raise even a single-CuO₂-plane cuprate past 90 K (Hg-1201 = 92–98), but Tl reaches high Tc ONLY with
multiple CuO₂ planes + a Ca spacer (Tl-2223=119 high; Tl-1201+Cd, single plane, no Ca = 88 low);
with no Hg/Tl at all the material is low even at 3 planes (Y-Ba-La-Cu3 = 81).
"Contains Hg" alone is WRONG because it mislabels Tl multi-plane+Ca cuprates (which are high) as low.

## UNMEASURABLE PREDICTIONS
1. O6.160Ca1Cu2Ba2Hg1 — Hg present → **high** (interp ~115 between 6.140@105 & 6.190@122)
2. O6.270Ca1Cu2Ba2Hg1 — Hg present → **high** (interp ~111 between 6.240@123 & 6.300@100)
3. O4.180Cu1Ba2Hg1   — Hg present → **high** (interp ~96 between 4.162@97.4 & 4.200@94.2)
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl + Cu2.2(≥2) + Ca0.85 → **high** (Tl-1212-like)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — Tl + Cu2(≥2) + Ca1 → **high** (Tl/Pb-1212-like, Sr≈Ba site)
6. O8Cu3Y1Ba1.900La0.100     — no Hg/Tl → **low** (YBCO-123 analog, 3 planes w/o Hg/Tl)
7. O8Cu4Y1Ba1.870La0.130     — no Hg/Tl → **low** (Y-124/247 analog)

## Step D — final pick
High predictions: {1,2,3,4,5}. Element counts:
- #1 O6.160Ca1Cu2Ba2Hg1 → {O,Ca,Cu,Ba,Hg} = 5
- #2 O6.270Ca1Cu2Ba2Hg1 → 5
- #3 O4.180Cu1Ba2Hg1    → {O,Cu,Ba,Hg} = **4** ← fewest (unique)
- #4 O8Ca0.850Cu2.200Ba2Tl1.800 → {O,Ca,Cu,Ba,Tl} = 5
- #5 O7Ca1Cu2Sr2Tl0.500Pb0.500 → {O,Ca,Cu,Sr,Tl,Pb} = 6
**FINAL = O4.180Cu1Ba2Hg1** (4 elements, uniquely fewest; no tie to break).
