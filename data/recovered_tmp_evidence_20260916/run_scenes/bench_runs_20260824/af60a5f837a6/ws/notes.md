# Notes

## Setup
- Budget: 12 measurements total, shared across Task 1 + Task 2.
- m05 = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain_len 11), never synthesized → probe_unavailable. Predict from its series.
- m05 scaffold = `COc1ccc(cc1)C(=O)N` (anisoyl amide). Members by chain_len:
  - m15 (2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)

## Task 1 — predict m05 QED from anisoyl series

Measured all 5 available anisoyl members (chain_len → QED):
- m15 (len2, rotb3): 0.7621
- m24 (len3, rotb4): 0.7919  ← peak
- m28 (len4, rotb5): 0.7518
- m17 (len5, rotb6): 0.7498
- m22 (len6, rotb7): 0.7377

Fits at chain_len=11:
- degree 2 → 0.5416  (RMSE 0.0119) ← chosen
- degree 1 → 0.6950  (RMSE 0.0131)

### Why degree-2 (the lower, curved value) is the right extrapolation
Qed_components breakdown of the 5 measured members shows which desirability scores
actually move with chain length (HBA, HBD, PSA, AROM are constant within a scaffold):
- MW score RISES with chain_len (0.33→0.68) as mw approaches optimal ~300 (m05 mw=305, ~peak)
- ALOGP score rises then plateaus ~0.997 at len5-6; m05 alogp=4.956 (high) → falls
- ROTB score FALLS steeply: 0.99 (rotb3) → 0.53 (rotb7); m05 rotb=12 → collapses toward ~0
- ALERTS steps to 0.6397 at len≥4, plateaus

Component-level fits at len=11:
- MW  deg2 → 1.31 (saturates/caps at 1; mw already near optimal)
- ALOGP deg2 → 0.684 (alogp 4.956 high side)
- ROTB deg2 → -1.05, deg1 → -0.034 (i.e. collapses to ~0)
A long N-alkyl chain mainly kills ROTB (and nudges ALOGP down). QED combines the 8
desirability scores via a (weighted) geometric mean, so one score collapsing to ~0 drags
the overall down superlinearly. That is exactly the curvature the degree-2 overall fit
captures (and deg2's RMSE is lower than deg1's). So m05 QED ≈ **0.5416**.

## Task 2 — pick a lead (QED≥0.6, psa≥40, not anisoyl scaffold)

psa by scaffold: nitro=72.24 ✓, pentahydroxy=110.02 ✓, biphenyl=29.1 ✗ (excluded).

Measurements (Task 2 candidates):
- m14 (pentahydroxy len2): 0.3237 ✗
- m27 (pentahydroxy len3): 0.3330 ✗  → pentahydroxy series too polar, poor
- m09 (nitro len2): 0.5826 ✗ (just under; ALERTS component 0.2414 = nitro alert)
- m12 (nitro len4): 0.4709 ✗
- m32 (nitro len5): 0.4683 ✗
- m02 (nitro len3): 0.6046 ✓  ← meets all three constraints

### m02 = `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ anisoyl; not same series as m05 ✓

Budget used: 11/12 (1 spare). All reported measured QED values come from real measure_qed calls.

## Answer
- target_qed (m05) = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046
