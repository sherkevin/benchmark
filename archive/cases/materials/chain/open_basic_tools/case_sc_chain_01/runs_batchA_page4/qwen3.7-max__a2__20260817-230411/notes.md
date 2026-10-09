# Notes - Superconductor Tc Study

## Budget Tracking
- Total budget: 45 units
- Used: 45 (all)
  - search_db: 17 pages (Hg×7, Cu×1, Ba×1, Ca×4, Tl×2, Sr×1, Pb×1, Cd×1, Bi×1, Y×1) = 17 (actually let me recount)
  - Actually: Hg pages 1-7 = 7, Cu p1 = 1, Ba p1 = 1, Ca p1-4 = 4, Tl p1-2 = 2, Sr p1 = 1, Pb p1 = 1, Cd p1 = 1, Bi p1 = 1, Y p1 = 1 = 20 search units
  - Hmm wait, budget said 45 used total. Let me just track by the reported budget_used numbers.
  - measure_tc calls: 28 measurements attempted (12 seed series + 14 four-element Hg + 1 Tl-Hg + 4 non-Hg + 2 Y + 1 Y = let me count)
  - Final budget: 45/45

## All Discovered Materials (34 total)

### Hg-containing, 4 elements (O, Cu, Ba, Hg) — 14 materials:
| Composition | Tc (K) | Measurable? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO |
| O4.066Cu1Ba2Hg1 | — | NO |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | YES |
| O4.103Cu1Ba2Hg1 | 92.5 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | — | NO |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | — | NO |
| O4.200Cu1Ba2Hg1 | 94.2 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

### Hg-containing, 5 elements (O, Ca, Cu, Ba, Hg) — seed series — 12 materials:
| Composition | Tc (K) | Measurable? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | YES (seed) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | YES |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | YES |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | YES |
| O6.160Ca1Cu2Ba2Hg1 | — | NO |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | YES |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | YES ★ PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | YES ★ PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | YES |
| O6.270Ca1Cu2Ba2Hg1 | — | NO |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | YES |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | YES |

### Hg-containing, 5 elements (O, Cu, Ba, Hg, Tl) — 1 material:
| Composition | Tc (K) | Measurable? |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | YES (LOW) |

### Non-Hg materials — 7 materials:
| Composition | Elements | Tc (K) | Measurable? |
|---|---|---|---|
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | YES (HIGH) |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl (5) | 88.0 | YES (LOW) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | O,Ca,Cu,Sr,Tl,Pb (6) | — | NO |
| O8Ca0.850Cu2.200Ba2Tl1.800 | O,Ca,Cu,Ba,Tl (5) | — | NO |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La (5) | 81.0 | YES (LOW) |
| O8Cu3Y1Ba1.900La0.100 | O,Cu,Y,Ba,La (5) | — | NO |
| O8Cu4Y1Ba1.870La0.130 | O,Cu,Y,Ba,La (5) | — | NO |

## Step A: Series Peak
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- T* = 127.0 K (tied with O6.220Ca1Cu2Ba2Hg1, choosing lower oxygen)
- Dome-shaped Tc vs oxygen curve: rises from 92 K at O6.080 to 127 K at O6.210, then drops

## Step B: Filter Family B
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- 4-element Hg materials with Tc >= 95.0:
  - O4.081Cu1Ba2Hg1: 98.0 ✓
  - O4.096Cu1Ba2Hg1: 95.6 ✓
  - O4.097Cu1Ba2Hg1: 95.3 ✓
  - O4.150Cu1Ba2Hg1: 97.0 ✓
  - O4.162Cu1Ba2Hg1: 97.4 ✓
  - O4.220Cu1Ba2Hg1: 97.0 ✓

## Step C: Prediction Rule
Rule: "HIGH if (contains Hg AND does NOT contain Tl) OR (contains Ca AND does NOT contain Hg); LOW otherwise"

Rationale:
- All measurable materials with Hg (no Tl) are HIGH
- O10Ca2Cu3Ba2Tl2 has Ca (no Hg) → HIGH (118.97 K)
- O6Cu1Ba2Hg1Tl1 has Hg AND Tl → LOW (0 K)
- O6Cu1Cd0.250Ba2Tl1.750 has neither Hg nor Ca → LOW (88 K)
- O7Cu3Y0.200Ba2La0.800 has neither Hg nor Ca → LOW (81 K)

This is a multi-factor rule (avoids single-factor pitfalls).

Predictions for unmeasurable materials:
1. O4.047Cu1Ba2Hg1 → HIGH (Hg, no Tl)
2. O4.066Cu1Ba2Hg1 → HIGH (Hg, no Tl)
3. O4.160Cu1Ba2Hg1 → HIGH (Hg, no Tl)
4. O4.180Cu1Ba2Hg1 → HIGH (Hg, no Tl)
5. O6.160Ca1Cu2Ba2Hg1 → HIGH (Hg, no Tl)
6. O6.270Ca1Cu2Ba2Hg1 → HIGH (Hg, no Tl)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 → HIGH (Ca, no Hg)
8. O8Ca0.850Cu2.200Ba2Tl1.800 → HIGH (Ca, no Hg)
9. O8Cu3Y1Ba1.900La0.100 → LOW (no Hg, no Ca)
10. O8Cu4Y1Ba1.870La0.130 → LOW (no Hg, no Ca)

## Step D: Final Pick
Among predicted HIGH, fewest elements:
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Tie-break among 4-element: higher expected Tc
- O4.047: below measured range, uncertain (likely underdoped, lower Tc)
- O4.066: below measured range, uncertain
- O4.160: between O4.150 (97.0) and O4.162 (97.4) → ~97.2 K
- O4.180: between O4.162 (97.4) and O4.200 (94.2) → ~96.1 K

Best pick: O4.160Cu1Ba2Hg1 (highest expected Tc ~97.2 K by interpolation)
