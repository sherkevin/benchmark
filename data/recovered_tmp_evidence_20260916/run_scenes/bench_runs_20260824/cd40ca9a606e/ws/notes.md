# Predicting QED of m05 (4-methoxy-N-undecylbenzamide)

## Target
- `m05` = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (N-undecyl, 11-carbon N-alkyl chain).
- `measure_qed` returns `probe_unavailable` (never synthesized) → must predict.

## 1. Finding the series (same scaffold, varying N-alkyl chain)
Scaffold substring `COc1ccc(cc1)C(=O)N` of the target was searched. This is the
p-anisamide headgroup; the tail `C...C` after the amide N is what varies.

`search_pool` returned 6 total matches across 2 pages (cost 2 budget):

| mol_id | SMILES tail (after N) | N-alkyl chain C | rotb | mw    | alogp |
|--------|-----------------------|------------------|------|-------|-------|
| m15    | CC                    | 2 (ethyl)        | 3    | 179.2 | 1.445 |
| m24    | CCC                   | 3 (propyl)       | 4    | 193.2 | 1.835 |
| m28    | CCCC                  | 4 (butyl)        | 5    | 207.3 | 2.225 |
| m17    | CCCCC                 | 5 (pentyl)       | 6    | 221.3 | 2.615 |
| m22    | CCCCCC                | 6 (hexyl)        | 7    | 235.3 | 3.005 |
| m05    | CCCCCCCCCCC           | 11 (undecyl)     | 12   | 305.5 | 4.956 |  ← target (unmeasurable)

All share PSA=38.33, arom=1 (one anisole ring). `rotb` = chain_C + 1 (the amide C-N
bond is rotatable too), so chain length is a clean 1-indexed ladder. I use `rotb`
as the independent variable since it's directly tied to chain length.

## 2. Measured QEDs (5 real `measure_qed` calls, cost 5 budget; total 7/12)

| mol_id | rotb | QED    |
|--------|------|--------|
| m15    | 3    | 0.7621 |
| m24    | 4    | 0.7919 |  ← peak
| m28    | 5    | 0.7518 |
| m17    | 6    | 0.7498 |
| m22    | 7    | 0.7377 |

Observed shape: QED rises from rotb=3→4 (ethyl→propyl) then declines
monotonically from rotb=4→7. Target m05 sits at rotb=12, **5 steps beyond the
measured range**, deep in the declining tail. Extrapolation (not interpolation)
of the downward trend is required.

## 3. Trend fitting

QED is a bounded [0,1] drug-likeness score. As a long alkyl chain grows,
molecular weight, ALogP and rotatable bonds all climb steadily (rotb is a linear
proxy for chain length: mw ≈ 14·rotb+137, alogp ≈ 0.39·rotb+0.27). QED penalizes
these (MW, ALogP, rotb are three of QED's eight weighted properties), so the
late-tail QED should keep *declining* with chain length, at a rate set by how
fast those penalties accumulate — not flatten to a constant, and not a steep
cliff (the underlying properties move linearly, so QED's penalty ramps roughly
linearly per chain atom in this regime).

**Model.** Fit the four declining points (rotb 4→7) to a line in rotb, then
extrapolate the same slope down to rotb=12. (Only the post-peak tail carries the
signal for where m05 lives; including the pre-peak rising point m15 would pull
the slope the wrong way.)

Points fitted (rotb, QED): (4, 0.7919), (5, 0.7518), (6, 0.7498), (7, 0.7377).

Linear regression slope/intercept:
- mean_x = 5.5, mean_y = (0.7919+0.7518+0.7498+0.7377)/4 = 0.7578
- Sxy = Σ(x-x̄)(y-ȳ) = (-1.5)(0.0341)+(-0.5)(-0.0060)+(0.5)(-0.0080)+(1.5)(-0.0201)
      = -0.05115 + 0.0030 - 0.0040 - 0.03015 = -0.0823
- Sxx = (-1.5)²+(-0.5)²+(0.5)²+(1.5)² = 5.0
- slope b = -0.0823 / 5.0 = -0.01646 per rotb
- intercept a = 0.7578 - (-0.01646)(5.5) = 0.7578 + 0.0905 = 0.8483

Fit: QED ≈ 0.8483 - 0.01646·rotb.
Check at data: rotb=4 → 0.7825, 5 → 0.7660, 6 → 0.7495, 7 → 0.7331 (close to
measured, captures the gentle decline).

## 4. Extrapolation to m05 (rotb = 12)
QED(m05) ≈ 0.8483 - 0.01646·12 = 0.8483 - 0.1975 = **0.6508**

Plausibility checks against the QED mechanics:
- This slope (≈ -0.0165/rotb) is mild per carbon, consistent with QED's
  piecewise-continuous, saturating penalty functions rather than a hard cutoff.
- At rotb=12 the molecule is still single-aromatic, low PSA, no flagged
  functional groups — QED won't collapse toward 0; a value near 0.65 (still
  "drug-like-ish", below the 0.736 of the longest measured member m22) is
  physically reasonable for an undecyl anisamide whose ALogP≈5.0 and MW≈305 push
  a couple of QED sub-scores into their penalty zones but not catastrophically.
- It is well below the lowest measured value (0.7377), correctly extrapolating
  the decline rather than anchoring to the nearest measured molecule.

**Predicted QED(m05) = 0.651** (rounded to 3 dp from 0.6508).

## Budget audit
- search_pool pages: 2  (budget 2)
- measure_qed calls: 5 (m15, m24, m28, m17, m22) (budget 5)
- Total used: 7 of 12. ✓

## congener_ids (same series as m05, all 5 measurable + target excluded)
The deliverable asks for pool molecules found in the same series. m05 is the
target (unmeasurable, not a pool molecule to "cite as measured"); the 5 measured
congeners are m15, m24, m28, m17, m22.
