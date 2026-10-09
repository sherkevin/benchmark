# Superconductor Tc study — notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg → 5 elements)
- High = Tc >= 90 K, Low = Tc < 90 K
- Budget: 34 measure_tc calls. Used: 26.

## Measured Tc values

### Series (Ca1Cu2Ba2Hg1, varying O) — Step A
| O | Tc |
|---|---|
| 6.080 | 92.0 |
| 6.100 | 92.5 |
| 6.130 | 102.0 |
| 6.140 | 105.0 |
| 6.190 | 122.0 |
| 6.210 | **127.0** ← max (tied) |
| 6.220 | **127.0** ← max (tied) |
| 6.240 | 123.0 |
| 6.300 | 100.0 |
| 6.310 | 98.0 |

T* = 127.0. Tie between O6.210 and O6.220 (flat peak plateau).
Picking **O6.210** as peak (first occurrence of the maximum). T*=127.0 is unambiguous
and is what drives threshold/Q1.

### O4.xCu1Ba2Hg1 series (4 elements: O,Cu,Ba,Hg; contains Hg) — Step B candidates
| O | Tc | ≥95.0? |
|---|---|---|
| 4.081 | 98.0 | ✓ Q1 |
| 4.162 | 97.4 | ✓ Q1 |
| 4.150 | 97.0 | ✓ Q1 |
| 4.220 | 97.0 | ✓ Q1 |
| 4.096 | 95.6 | ✓ Q1 |
| 4.097 | 95.3 | ✓ Q1 |
| 4.047 | 95.0 | ✓ Q1 (exactly = threshold) |
| 4.200 | 94.2 | ✗ |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.085 | 92.0 | ✗ |

### Other measured
| Composition | Elems | Hg? | Tc | High? |
|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 5 (O,Cu,Ba,Hg,Tl) | Y | 0.0 | L |
| O10Ca2Cu3Ba2Tl2 | 5 (O,Ca,Cu,Ba,Tl) | N | 118.97 | H |
| O6Cu1Cd0.250Ba2Tl1.750 | 5 (O,Cu,Cd,Ba,Tl) | N | 88.0 | L |
| O7Cu3Ba2La1 | 4 (O,Cu,Ba,La) | N | 89.6 | L |
| O7Cu3Y0.200Ba2La0.800 | 5 (O,Cu,Y,Ba,La) | N | 81.0 | L |

## Step A answer
- peak = O6.210Ca1Cu2Ba2Hg1, peak_tc = T* = 127.0

## Step B answer
- threshold1 = T* − 32 = 127.0 − 32 = **95.0**
- Q1 (contains Hg ∧ exactly 4 elements ∧ Tc ≥ 95.0) =
  [O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047]Cu1Ba2Hg1 (7 members)
- Note: only the O4.xCu1Ba2Hg1 line qualify — series 1-10 have 5 elements
  (excluded); lines 22+ either lack Hg or have ≠4 elements.

## Step C — rule induction
Single-factor checks fail:
- "contains Hg" wrong: O6Cu1Ba2Hg1Tl1 has Hg but Tc=0 (Hg+Tl mix kills it).
- "has 5 elements" wrong: 5-elem set has both high (Hg series, O10..Tl2) and low
  (Hg1Tl1, Cd-Tl, Y-La).

Induced multi-factor rule (verified on ALL 26 measured):
**High iff [contains Hg AND NOT contains Tl] OR [contains Tl AND contains Ca AND NOT contains Hg].**
- Hg-cuprates w/o Tl → high (all O6.xCa1Cu2Ba2Hg1 92–127; all O4.xCu1Ba2Hg1 92–98).
- Hg+Tl together → low (O6Cu1Ba2Hg1Tl1 = 0).
- Tl-cuprate needs Ca → high only with Ca (O10Ca2Cu3Ba2Tl2 = 118.97);
  Tl w/o Ca → low (O6Cu1Cd0.250Ba2Tl1.750 = 88.0).
- No Hg and no Tl → low (La, Y/La cuprates here: 89.6, 81.0).

Predictions for unmeasurable.txt:
| Composition | Hg? | Tl? | Ca? | Rule → |
|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | Y | N | Y | HIGH (Hg∧¬Tl) |
| O6.270Ca1Cu2Ba2Hg1 | Y | N | Y | HIGH |
| O4.180Cu1Ba2Hg1 | Y | N | N | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | N | Y | Y | HIGH (Tl∧Ca∧¬Hg) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | N | Y | Y | HIGH (Tl∧Ca∧¬Hg; Pb/Sr don't break it) |
| O8Cu3Y1Ba1.900La0.100 | N | N | N | LOW |
| O8Cu4Y1Ba1.870La0.130 | N | N | N | LOW |

(None of the unmeasurable has both Hg and Tl, so the Hg+Tl exclusion is not
exercised on predictions — but it's the key insight from measured data.)

## Step D — final pick
Predicted-high unmeasurable and element counts:
- O6.160Ca1Cu2Ba2Hg1 → 5
- O6.270Ca1Cu2Ba2Hg1 → 5
- O4.180Cu1Ba2Hg1 → **4** (fewest)
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O7Ca1Cu2Sr2Tl0.500Pb0.500 → 6

Unique minimum → final = **O4.180Cu1Ba2Hg1** (no tie-break needed).
(Expected Tc ~96 K, interpolating O4.162=97.4 / O4.200=94.2 — comfortably high.)
