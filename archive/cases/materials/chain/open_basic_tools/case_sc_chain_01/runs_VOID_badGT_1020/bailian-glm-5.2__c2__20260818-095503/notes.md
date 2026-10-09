# Superconductor Tc Study — Notes

## Task summary
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O, Ca, Cu, Ba, Hg)
- High = Tc >= 90 K, Low = Tc < 90 K
- Step A: homologous doping series (same element set, varying O), find MAX Tc = T*
- Step B: threshold1 = T* - 32. Materials containing Hg AND exactly 4 elements AND Tc >= threshold1 = Q1
- Step C: For unmeasurable materials (no sample), induce a rule, predict high/low for each
- Step D: Among predicted-high unmeasurable materials, choose FEWEST elements (tie-break higher expected Tc)

KEY BUDGET FACT: measure_tc on unmeasurable (no sample) materials is FREE (budget not incremented). Only successful measurements cost.

## Step A — Series (Ca1Cu2Ba2Hg1, varying O) — all 12 members measured
| Composition | O | Tc | Sample? |
|-------------|------|------|---------|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | NULL | NO SAMPLE (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | 127.0 | yes |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | 127.0 | yes |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | NULL | NO SAMPLE (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | yes |

Measured Tc curve vs O: rises 6.080→6.220 (peak 127 at 6.210-6.220), then declines.
Peak (max measured) = 127.0 at O6.210 AND O6.220. T* = 127.0.

## Step B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need materials with Hg AND exactly 4 elements AND measured Tc >= 95.0.

### 4-element Hg family found: O4.xCu1Ba2Hg1 (O, Cu, Ba, Hg — 4 elements) — 14 members
| Composition | O | Tc | Sample? | >=95? |
|-------------|------|------|---------|-------|
| O4.047Cu1Ba2Hg1 | 4.047 | NULL | NO | unmeasurable |
| O4.066Cu1Ba2Hg1 | 4.066 | NULL | NO | unmeasurable |
| O4.081Cu1Ba2Hg1 | 4.081 | 98.0 | yes | YES |
| O4.085Cu1Ba2Hg1 | 4.085 | 92.0 | yes | no |
| O4.096Cu1Ba2Hg1 | 4.096 | 95.6 | yes | YES |
| O4.097Cu1Ba2Hg1 | 4.097 | 95.3 | yes | YES |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | yes | no |
| O4.103Cu1Ba2Hg1 | 4.103 | 92.5 | yes | no |
| O4.150Cu1Ba2Hg1 | 4.150 | 97.0 | yes | YES |
| O4.160Cu1Ba2Hg1 | 4.160 | NULL | NO | unmeasurable |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | yes | YES |
| O4.180Cu1Ba2Hg1 | 4.180 | NULL | NO | unmeasurable |
| O4.200Cu1Ba2Hg1 | 4.200 | 94.2 | yes | no |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | yes | YES |

Q1 (4-element, Hg, measurable, Tc>=95.0):
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

(Unmeasurable 4-element ones — O4.047, O4.066, O4.160, O4.180 — are NOT in Q1 because Step B is among measurable materials with known Tc >= threshold1. They go to Step C for prediction.)

## Step C — Unmeasurable materials (no sample) found via search_db
1. O6.160Ca1Cu2Ba2Hg1 (5 el, O=6.160) — series member
2. O6.270Ca1Cu2Ba2Hg1 (5 el, O=6.270) — series member
3. O4.047Cu1Ba2Hg1 (4 el, O=4.047)
4. O4.066Cu1Ba2Hg1 (4 el, O=4.066)
5. O4.160Cu1Ba2Hg1 (4 el, O=4.160)
6. O4.180Cu1Ba2Hg1 (4 el, O=4.180)
(also O6Cu1Ba2Hg1Tl1 (5 el) — never measured, budget exhausted; treat as discovered-but-unmeasured)

### Rule induction
Series A (5-el) measured Tc vs O (ascending):
6.080→92.0, 6.100→92.5, 6.130→102.0, 6.140→105.0, [6.160?], 6.190→122.0, 6.210→127.0, 6.220→127.0, 6.240→123.0, [6.270?], 6.300→100.0, 6.310→98.0.
Smooth peak. Interpolation:
- 6.160 between 105 (6.140) and 122 (6.190): ~113.5-115 K → HIGH (>=90).
- 6.270 between 123 (6.240) and 100 (6.300): ~111 K → HIGH (>=90).

Family B (4-el) measured Tc vs O (ascending):
4.047→[?], 4.066→[?], 4.081→98.0, 4.085→92.0, 4.096→95.6, 4.097→95.3, 4.101→93.4, 4.103→92.5, 4.150→97.0, 4.160→[?], 4.162→97.4, 4.180→[?], 4.200→94.2, 4.220→97.0.
In the measured range O=4.081 to 4.220, Tc stays 92-98 K — ALL HIGH (>=90). The 4-el Hg cuprates hold Tc in low/mid-90s across the whole measured range. No low data point exists in measured range.
- 4.047 and 4.066 are just below 4.081 (=98.0). Near neighbors are high. Predict HIGH.
- 4.160 between 97.0 (4.150) and 97.4 (4.162): ~97 K → HIGH.
- 4.180 between 97.4 (4.162) and 94.2 (4.200): ~96 K → HIGH.

### Rule (text): Tc of Hg-based cuprate is governed by O content within the compositional family (bell curve around the family optimum); interpolate within the measured range and extrapolate gently. Unmeasurable points were predicted by linear interpolation between their nearest measured neighbors within the same element family — every unmeasurable point sits between measured points that are all HIGH (>=90 K), so all are predicted HIGH. (No single-factor rule on one variable fits all families; family-relative interpolation is needed.)

## Step D
Predicted-high unmeasurable materials and element counts:
- O6.160Ca1Cu2Ba2Hg1 — 5 elements, ~113 K
- O6.270Ca1Cu2Ba2Hg1 — 5 elements, ~111 K
- O4.047Cu1Ba2Hg1 — 4 elements, ~98 K (extrapolated from 4.081)
- O4.066Cu1Ba2Hg1 — 4 elements, ~98 K
- O4.160Cu1Ba2Hg1 — 4 elements, ~97 K
- O4.180Cu1Ba2Hg1 — 4 elements, ~96 K
Fewest elements = 4. Tie-break higher expected Tc among the 4-element ones:
- O4.047 ~98 K (just below 4.081=98)
- O4.066 ~98 K
- O4.160 ~97 K (between 97.0 and 97.4)
- O4.180 ~96 K
Highest expected: O4.047 and O4.066 both ~98 K. Tie between them.
O4.047 is the lowest-O measured-adjacent (4.081=98.0); O4.066 also ~98.
Pick O4.047Cu1Ba2Hg1 as final (4 elements, highest expected Tc ~98 K, lowest O).
(Tie-break is by higher expected Tc; both ~98, so either acceptable. Choose O4.047Cu1Ba2Hg1.)

## Budget
- Total 34, all used (34/34).
