# Notes — QED prediction and lead selection

## Pool structure (free, from `describe_structure` + reading SMILES)

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` → scaffold `COc1ccc(cc1)C(=O)N`, chain_len = 11.
The pool members sharing that scaffold (m05's **own congeneric series**) are:

| mol_id | chain_len | psa   | measured QED |
|--------|-----------|-------|--------------|
| m15    | 2         | 38.33 | 0.7621       |
| m24    | 3         | 38.33 | 0.7919       |
| m28    | 4         | 38.33 | 0.7518       |
| m17    | 5         | 38.33 | 0.7498       |
| m22    | 6         | 38.33 | 0.7377       |
| m05    | 11        | 38.33 | unmeasurable (target) |

So the pool only holds the **short-chain** members (chains 2–6); m05 at chain 11 is beyond the
measured range → this is **extrapolation**, not interpolation.

## Job 1 — predicting m05's QED

Measured the five congeners (budget 1–5). Trend: QED rises from chain 2 (0.7621) to a peak at
chain 3 (0.7919), then declines (0.7518 → 0.7498 → 0.7377). It is clearly **not** a straight line,
and the task explicitly warns against assuming linearity.

Fit with `fit_trend` (xs = chain lengths, ys = measured QED, x_query = 11):

- **Degree 2 (quadratic):** prediction **0.5416**
  coeffs (low→high) = [0.74932, 0.017024, −0.003264], RMSE = 0.01188
- Degree 1 (linear): prediction 0.695
  coeffs = [0.79502, −0.00909], RMSE = 0.01308

The quadratic has the **lower RMSE** (0.01188 < 0.01308) and matches the observed curvature
(negative x² term = downward parabola). The linear fit ignores the peak at chain 3 and is
explicitly cautioned against. → **m05 QED prediction = 0.5416** (quadratic extrapolation to chain 11).

Extrapolation is less certain than interpolation, so the quadratic's downward-curving trajectory
beyond chain 6 is the best-supported estimate; the true value is expected to sit below the
linear projection (0.695) and near 0.54.

## Job 2 — backup lead compound

Three constraints: (1) measured QED ≥ 0.6, (2) psa ≥ 40, (3) not in m05's series.

Free checks first (`screen_candidates` + reading SMILES): 18 molecules pass psa ≥ 40.
Excluded m05's own series (m15/m24/m28/m17/m22, all psa 38.33 — below 40 anyway, and they're the
target's series). Also excluded the biphenyl series (psa 29.1 < 40).

Strategy: pick from the series **structurally closest to m05's** (so its QED curve sits high and is
most likely to clear 0.6). The nitro-benzamide series `O=[N+]([O-])c1ccc(cc1)C(=O)N` is the closest
para-substituted benzamide to the methoxy series, and its psa (72.24) ≥ 40. Measured its
short-chain members (budget 6–8):

| mol_id | chain_len | psa   | measured QED | ≥0.6? |
|--------|-----------|-------|--------------|-------|
| m09    | 2         | 72.24 | 0.5826       | no    |
| m02    | 3         | 72.24 | 0.6046       | **yes** |
| m12    | 4         | 72.24 | 0.4709       | no    |

**m02** passes all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N` → different series ✓

m02 is the lead. (Stopped at 8/12 budget; no need to spend more — m02 already clears every gate.)

## Deliverable values

- target_qed = 0.5416 (quadratic extrapolation of m05's series to chain 11)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids = all pool molecules in m05's series: m05, m15, m17, m22, m24, m28
