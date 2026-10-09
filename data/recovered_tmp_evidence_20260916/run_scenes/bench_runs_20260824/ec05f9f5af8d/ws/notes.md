# Task notes

## Setup
- Pool: 33 molecules, 4 scaffolds (congeneric series), each varying only in N-alkyl chain length.
- Budget: 12 measurements total, shared by Task 1 + Task 2.

## Scaffolds (from group_by_scaffold) — catalogue psa per scaffold
| scaffold | psa | members (chain: mol) |
|---|---|---|
| methoxy `COc1ccc(cc1)C(=O)N` | 38.33 | 2:m15, 3:m24, 4:m28, 5:m17, 6:m22, **11:m05 (target, unsynth)** |
| nitro `O=[N+]([O-])c1ccc(cc1)C(=O)N` | 72.24 | 2:m09, 3:m02, 4:m12, 5:m32, 6:m33, 7:m29, 8:m31, 9:m06, 10:m10 |
| sugar `OCC(O)C(O)C(O)C(=O)N` | 110.02 | 2:m14, 3:m27, 4:m01, 5:m19, 6:m03, 7:m21, 8:m18, 9:m07, 10:m25 |
| biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | 29.10 | 2:m13, 3:m11, 4:m04, 5:m30, 6:m23, 7:m20, 8:m26, 9:m08, 10:m16 |

Key structural fact: **m05's methoxy series has psa=38.33 < 40**, so EVERY methoxy-series member fails Task 2 constraint #2. This cleanly separates the two tasks — the methoxy measurements serve only Task 1; Task 2 must come from the nitro or sugar series (psa≥40).

## Task 1 — predict QED of m05 (methoxy, chain 11, unsynthesized → probe_unavailable)
Task says predict from the trend of its OWN congeneric series. Measured methoxy series:
- c2 m15 = 0.7621
- c3 m24 = 0.7919  ← peak
- c4 m28 = 0.7518
- c5 m17 = 0.7498
- c6 m22 = 0.7377
Trend: peak at c3, then gentle decline. Per-chain declines c3→c6: 0.040, 0.002, 0.012 (avg ~0.0165/chain).

Fits for x_query=11 (free tool):
- linear on c3–c6 tail (the decline regime) → **0.6508**, RMSE 0.00883 ← chosen
- quadratic on all 5 pts → 0.5416, RMSE 0.01188 (over-fits c2→c3 rise; projects downturn-acceleration the c3→c6 tail contradicts — tail declines are decelerating 0.040→0.002→0.012, not accelerating)
- quadratic on c3–c6 → 0.9378 (absurd upturn → degree-2 unstable on the tail)
- linear extrapolation sanity check: 0.7377 − 5×0.0165 ≈ 0.655, matches 0.6508.

Decision: the c3–c6 descending tail is the cleanest, most direct signal of the per-chain decline rate, and a linear fit on it is the most defensible single-method extrapolation. The c2 outlier (below the c3 peak) is a different regime (approaching optimum) that, if included in a parabola, falsely imposes a downturn-acceleration the actual tail data refute. **target_qed = 0.65.**

Uncertainty band: the quadratic-all-5 (0.54) and linear-tail (0.65) bracket the truth; the tail's decelerating shape and the instruction to use the OWN-series trend both favor the higher, linear estimate.

## Task 2 — pick a lead (QED≥0.6 AND psa≥40 AND not methoxy series)
psa≥40 ⇒ only nitro (72.24) or sugar (110.02) series qualify (methoxy 38.33 and biphenyl 29.10 excluded).
Measured candidates:
| mol | series | chain | qed | psa≥40 | not-methoxy | qed≥0.6 | verdict |
|---|---|---|---|---|---|---|---|
| m09 | nitro | 2 | 0.5826 | ✓ | ✓ | ✗ (0.583) | fail |
| **m02** | nitro | 3 | **0.6046** | ✓ (72.24) | ✓ | ✓ (0.605) | **PASS (all 3)** |
| m12 | nitro | 4 | 0.4709 | ✓ | ✓ | ✗ | fail |
| m32 | nitro | 5 | 0.4683 | ✓ | ✓ | ✗ | fail |
| m06 | nitro | 9 | 0.4011 | ✓ | ✓ | ✗ | fail |
| m10 | nitro | 10 | 0.3724 | ✓ | ✓ | ✗ | fail |
| m01 | sugar | 4 | 0.3187 | ✓ | ✓ | ✗ | fail |

Nitro series behavior (incidental calibration): cliff c3→c4 (0.605→0.471), then plateau to c10 (0.37). Different decline shape from methoxy's gentle tail — confirms one should NOT borrow nitro's shape for the methoxy prediction; own-series trend is the right basis (as task instructs).

m02 is the only measured molecule passing all three Task 2 constraints. **lead = m02, measured QED = 0.6046.**

m02 was measured directly via measure_qed (measurement #7), so lead_qed_measured is a real measured value as required.

## Measurements log (12/12 used)
| # | mol | series | chain | qed |
|---|---|---|---|---|
| 1 | m15 | methoxy | 2 | 0.7621 |
| 2 | m24 | methoxy | 3 | 0.7919 |
| 3 | m28 | methoxy | 4 | 0.7518 |
| 4 | m17 | methoxy | 5 | 0.7498 |
| 5 | m22 | methoxy | 6 | 0.7377 |
| 6 | m09 | nitro | 2 | 0.5826 |
| 7 | m02 | nitro | 3 | 0.6046 | ← Task 2 lead
| 8 | m12 | nitro | 4 | 0.4709 |
| 9 | m32 | nitro | 5 | 0.4683 |
| 10 | m06 | nitro | 9 | 0.4011 |
| 11 | m10 | nitro | 10 | 0.3724 |
| 12 | m01 | sugar | 4 | 0.3187 |

## Answers
- target_qed (m05) = 0.65
- lead_mol_id = m02
- lead_qed_measured = 0.6046
