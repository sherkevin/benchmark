# Notes - Superconductor Tc Study

## Budget Tracking
- Total budget: 45 units
- Used: 42 (7 search pages + 25 successful measurements + 10 failed attempts that were free)
- Remaining: 3

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`
- Elements: O, Ca, Cu, Ba, Hg (5 elements)

## All Materials Discovered (35 total via search_db)
Cu search confirms 35 total materials = 27 Hg-containing + 8 non-Hg

## Step A: Series Peak — Seed's Homologous Series (O-Ca-Cu-Ba-Hg, 5 elements)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measurable |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measurable |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measurable |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measurable |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measurable |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measurable |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measurable |

**Peak: T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (tied with O6.220, picking lower-O one)

## 4-element Hg materials (O-Cu-Ba-Hg, 14 materials)
| Composition | Tc (K) | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.066Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.081Cu1Ba2Hg1 | 98.0 | measurable |
| O4.085Cu1Ba2Hg1 | 92.0 | measurable |
| O4.096Cu1Ba2Hg1 | 95.6 | measurable |
| O4.097Cu1Ba2Hg1 | 95.3 | measurable |
| O4.101Cu1Ba2Hg1 | 93.4 | measurable |
| O4.103Cu1Ba2Hg1 | 92.5 | measurable |
| O4.150Cu1Ba2Hg1 | 97.0 | measurable |
| O4.160Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.162Cu1Ba2Hg1 | 97.4 | measurable |
| O4.180Cu1Ba2Hg1 | — | UNMEASURABLE |
| O4.200Cu1Ba2Hg1 | 94.2 | measurable |
| O4.220Cu1Ba2Hg1 | 97.0 | measurable |

## Other Hg material (5 elements)
| Composition | Tc (K) | Status |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | measurable (LOW) |

## Non-Hg materials (8 materials)
| Composition | Tc (K) | Elements | Status |
|---|---|---|---|
| O7Cu3Ba2La1 | 89.6 | O,Cu,Ba,La (4) | measurable (LOW) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | O,Cu,Y,Ba,La (5) | measurable (LOW) |
| O8Cu3Y1Ba1.900La0.100 | — | O,Cu,Y,Ba,La (5) | UNMEASURABLE |
| O8Cu4Y1Ba1.870La0.130 | — | O,Cu,Y,Ba,La (5) | UNMEASURABLE |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl (5) | measurable (HIGH) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | O,Cu,Cd,Ba,Tl (5) | measurable (LOW) |
| O8Ca0.850Cu2.200Ba2Tl1.800 | — | O,Ca,Cu,Ba,Tl (5) | UNMEASURABLE |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | — | O,Ca,Cu,Sr,Tl,Pb (6) | UNMEASURABLE |

## Step B: Q1 (Hg + exactly 4 elements + Tc >= 95.0)
threshold1 = 127.0 - 32 = 95.0
Qualifying (measurable, Tc >= 95.0):
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓

NOT qualifying (Tc < 95.0):
- O4.085Cu1Ba2Hg1: 92.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 ✗
- O4.103Cu1Ba2Hg1: 92.5 ✗
- O4.200Cu1Ba2Hg1: 94.2 ✗

## Step C: Rule Induction

### Measurable materials summary:
HIGH (Tc >= 90):
- All O-Cu-Ba-Hg (4-elem): 92.0 - 98.0 K (10 materials)
- All O-Ca-Cu-Ba-Hg (5-elem): 92.0 - 127.0 K (10 materials)
- O10Ca2Cu3Ba2Tl2 (5-elem): 118.97 K

LOW (Tc < 90):
- O6Cu1Ba2Hg1Tl1: 0.0 K (has both Hg AND Tl)
- O7Cu3Ba2La1: 89.6 K (has La)
- O7Cu3Y0.200Ba2La0.800: 81.0 K (has Y, La)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (has Cd)

### Induced Rule:
**A material is HIGH if it contains none of {La, Y, Cd} AND does not simultaneously contain both Hg and Tl. Otherwise LOW.**

Equivalently: LOW if (contains La) OR (contains Y) OR (contains Cd) OR (contains both Hg AND Tl).

Why single-factor rules fail:
- "Contains Hg → high": WRONG for O6Cu1Ba2Hg1Tl1 (has Hg but Tc=0)
- "Contains Tl → low": WRONG for O10Ca2Cu3Ba2Tl2 (has Tl but Tc=118.97)
- "Contains Ca → high": WRONG for O7Ca1Cu2Sr2Tl0.500Pb0.500 if we had simple expectations

### Predictions for all 10 unmeasurable materials:
1. O4.047Cu1Ba2Hg1 (4 elem) → no La/Y/Cd, no Tl → **HIGH**
2. O4.066Cu1Ba2Hg1 (4 elem) → no La/Y/Cd, no Tl → **HIGH**
3. O4.160Cu1Ba2Hg1 (4 elem) → no La/Y/Cd, no Tl → **HIGH**
4. O4.180Cu1Ba2Hg1 (4 elem) → no La/Y/Cd, no Tl → **HIGH**
5. O6.160Ca1Cu2Ba2Hg1 (5 elem) → no La/Y/Cd, no Tl → **HIGH**
6. O6.270Ca1Cu2Ba2Hg1 (5 elem) → no La/Y/Cd, no Tl → **HIGH**
7. O8Cu3Y1Ba1.900La0.100 (5 elem) → has Y, La → **LOW**
8. O8Cu4Y1Ba1.870La0.130 (5 elem) → has Y, La → **LOW**
9. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem) → no La/Y/Cd, has Tl but no Hg → **HIGH**
10. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem) → no La/Y/Cd, has Tl but no Hg → **HIGH**

## Step D: Final Pick
Predicted HIGH unmeasurable materials:
| Composition | Elements | Expected Tc |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 4 | ~93-95 (low-O extreme) |
| O4.066Cu1Ba2Hg1 | 4 | ~95-97 (approaching peak) |
| O4.160Cu1Ba2Hg1 | 4 | ~97 (between 97.0 and 97.4) |
| O4.180Cu1Ba2Hg1 | 4 | ~96 (between 97.4 and 94.2) |
| O6.160Ca1Cu2Ba2Hg1 | 5 | ~113 (between 105 and 122) |
| O6.270Ca1Cu2Ba2Hg1 | 5 | ~112 (between 123 and 100) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | ~100? |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | ~110? |

Fewest elements = 4 (four candidates).
Tie-break by higher expected Tc: O4.160Cu1Ba2Hg1 (~97, interpolated between 97.0 and 97.4).

**FINAL PICK: O4.160Cu1Ba2Hg1**
