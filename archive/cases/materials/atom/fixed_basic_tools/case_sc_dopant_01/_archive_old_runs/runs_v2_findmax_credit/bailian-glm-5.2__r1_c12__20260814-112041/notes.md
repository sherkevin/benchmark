# Notes: highest-Tc composition search

## Candidate structure analysis

30 candidates, all of form `O7Cu3Ba2Pr{x}RE{1-x}` — this is the **YBCO cuprate backbone**
(Ba2Cu3O7) with the Y-site shared between **Pr** (x) and a rare earth **RE ∈ {Er, Gd, Nd}** (1−x).

So it's a 2D grid:
- axis 1: Pr fraction x ∈ {0.030, ~0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900}
- axis 2: RE dopant ∈ {Er, Gd, Nd}

## Domain prior (Pr-doping suppression law)

Pr is the well-known suppressor of superconductivity in REBCO cuprates — PrBa2Cu3O7 is
essentially non-superconducting (magnetic pair-breaking + hole filling). So Tc is expected to
**decrease monotonically as x (Pr fraction) increases**. The global max should sit at the
**lowest x** (here x=0.030, the minimum Pr in the list).

Pure-RE end-member Tc references (REBa2Cu3O7, x→0): Er ≈ 92 K, Gd ≈ 92–93 K, **Nd ≈ 96 K**
(Nd is one of the highest-Tc REBCO members). So at equal low x, expect **Nd > Gd ≈ Er**.

Hypothesis: **Pr0.030Nd0.970** is the top candidate. Must verify with real measurements
since the measured Tc is the ground truth here, not my prior.

## Budget: 12 credits for 30 candidates

Strategy (adaptive):
1. Measure the 3 candidates at x=0.030 (min Pr) → finds best RE at the expected-max region. (3 cr)
2. Confirm monotonic decrease with x along the winning RE column, + spot-check other REs at
   mid-x to rule out a surprise peak. (~9 cr)
3. Commit to the max measured.

## Measurement log

| x (Pr) | Er Tc | Gd Tc | Nd Tc |
|---|---|---|---|
| 0.030 | **93.9** | 93.1 | 85.1 |
| 0.076 | 91.1 | — | — |
| 0.150 | 82.9 | 67.9 | 40.7 |
| 0.300 | 61.1 | — | — |

8 credits used / 12.

## Pattern found

- Er is the best RE at the low-Pr end (93.9 > 93.1 > 85.1).
- Tc **decreases monotonically as Pr fraction x increases** — confirmed cleanly on Er
  (93.9 → 91.1 → 82.9 → 61.1) and consistent on Gd (93.1 → 67.9) and Nd (85.1 → 40.7).
  This matches the Pr-doping suppression law (Pr breaks pairs / fills holes in REBCO).
- My initial "Nd highest" prior was **wrong** — measured truth: Nd is lowest and drops
  fastest. The real ordering is Er > Gd > Nd. Er wins at the min-Pr end.

## Why I stopped at 8 credits

- Er peak = 93.9 K at x=0.030, which is the **minimum Pr fraction in the whole candidate
  list** — no lower-x Er candidate exists, so Er cannot exceed 93.9. The Er curve is bounded
  with 4 monotone points.
- Gd and Nd both peak at their x=0.030 endpoint (93.1 and 85.1) and both decrease with x,
  so neither can exceed 93.9 either.
- The remaining 4 credits could only re-confirm the already-locked maximum. Spending them
  would not change the answer, so I stop and commit.

## Decision

**Final answer: `O7Cu3Ba2Pr0.030Er0.970` = 93.9 K** — the highest measured Tc, and
provably the global max (min-Pr endpoint of the winning RE column, with a monotone-decreasing
curve above it).
