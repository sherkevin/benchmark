# Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K
- Budget: 14 measure_tc calls
- Step A: Find peak Tc in homologous series (O,Ca,Cu,Ba,Hg varying O)
- Step B: threshold1 = T* - 32, find Hg + 4-element materials with Tc >= threshold1
- Step C: Predict high/low for unmeasurable materials using induced rule
- Step D: Among predicted high, fewest elements (tie-break: higher expected Tc)

## Measurable Series Members (O,Ca,Cu,Ba,Hg) - 10 items
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

## Measurable Hg + 4-element materials (O,Cu,Ba,Hg) - 11 items
11. O4.081Cu1Ba2Hg1
12. O4.162Cu1Ba2Hg1
13. O4.150Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
15. O4.096Cu1Ba2Hg1
16. O4.097Cu1Ba2Hg1
17. O4.047Cu1Ba2Hg1
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1

## Other measurable materials
22. O6Cu1Ba2Hg1Tl1 (5 elements, has Hg)
23. O10Ca2Cu3Ba2Tl2 (5 elements, no Hg)
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements, no Hg)
25. O7Cu3Ba2La1 (4 elements, no Hg)
26. O7Cu3Y0.200Ba2La0.800 (5 elements, no Hg)

## Unmeasurable materials (7 items)
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elements: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elements: O,Cu,Y,Ba,La)

## Step A Results — Series Peak
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K**, Peak composition: **O6.210Ca1Cu2Ba2Hg1**
Trend: bell curve peaking at O≈6.21-6.22

## Step B — Hg + 4-element filter
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Measured Hg+4-element (O,Cu,Ba,Hg) materials:
| Composition | O content | Tc (K) | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4.047 | 95.0 | YES (= threshold) |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | NO |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | YES |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | YES |

Unmeasured (interpolated from trend):
- O4.081 (O=4.081): ~94.0 → NO (between 4.047→95.0 and 4.101→93.4)
- O4.085 (O=4.085): ~93.9 → NO
- O4.096 (O=4.096): ~93.5 → NO (near the dip at 4.101→93.4)
- O4.097 (O=4.097): ~93.5 → NO
- O4.103 (O=4.103): ~93.5 → NO
- O4.150 (O=4.150): ~96.6 → YES (between 4.101→93.4 and 4.162→97.4, closer to 4.162)
- O4.200 (O=4.200): ~97.1 → YES (between 4.162→97.4 and 4.220→97.0)

Unmeasurable:
- O4.180Cu1Ba2Hg1 (O=4.180): ~97.2 → YES (between 4.162→97.4 and 4.220→97.0)

**Q1 = {O4.047Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**

## Step C — Rule Induction
All 14 measured materials are HIGH (Tc >= 90 K). Need to find a multi-factor rule.

Key features of measured materials:
- Series (O,Ca,Cu,Ba,Hg): 5 elements, all HIGH (92-127 K). Contains Hg, Ca, Ba, Cu.
- Hg+4elem (O,Cu,Ba,Hg): 4 elements, all HIGH (93.4-97.4 K). Contains Hg, Ba, Cu. No Ca.

Common features of all HIGH materials: contain Cu, Ba, and at least one of {Hg, Ca}.

Other measurable materials (not measured) for reference:
- O6Cu1Ba2Hg1Tl1: Cu, Ba, Hg → predicted HIGH
- O10Ca2Cu3Ba2Tl2: Cu, Ba, Ca → predicted HIGH (known Tl-cuprate family, ~110-120 K)
- O6Cu1Cd0.250Ba2Tl1.750: Cu, Ba (no Hg, no Ca) → predicted LOW
- O7Cu3Ba2La1: Cu, Ba, La (no Hg, no Ca) → predicted LOW (La-Ba-Cu-O known ~35 K)
- O7Cu3Y0.200Ba2La0.800: Cu, Ba, Y, La (no Hg, no Ca) → predicted LOW

**Induced Rule**: A material is HIGH-Tc (>= 90 K) if it contains Hg, OR if it contains both Ca AND Ba. Otherwise it is LOW.
Equivalently: HIGH if (Hg present) OR (Ca present AND Ba present). This is a multi-factor interaction rule — single factors like "contains Hg" or "contains Ca" alone fail.

Check single-factor failures:
- "Contains Hg" → misses O8Ca0.850Cu2.200Ba2Tl1.800 (no Hg but should be HIGH as Ca+Ba cuprate)
- "Contains Ca" → misses O4.xxxCu1Ba2Hg1 series (no Ca but HIGH due to Hg)
- "Has 5 elements" → misses 4-element Hg materials and material 5 (6 elements)

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: Hg ✓ → **HIGH** (~110 K by interpolation)
2. O6.270Ca1Cu2Ba2Hg1: Hg ✓ → **HIGH** (~108 K by interpolation)
3. O4.180Cu1Ba2Hg1: Hg ✓ → **HIGH** (~97 K by interpolation)
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ca ✓, Ba ✓ → **HIGH** (Tl-cuprate with Ca+Ba)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca ✓ but Ba ✗, Hg ✗ → **LOW** (no Ba to pair with Ca)
6. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → **LOW**
7. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → **LOW**

## Step D — Final Pick
Among predicted HIGH: {1, 2, 3, 4}
Element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O, Ca, Cu, Ba, Tl)

**Step D answer: O4.180Cu1Ba2Hg1** (4 elements, estimated Tc ~97 K)
