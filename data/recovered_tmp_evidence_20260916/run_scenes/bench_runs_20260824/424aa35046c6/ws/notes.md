# Notes — predict m05 QED and pick a backup lead

## Pool structure (free, via `describe_structure` on SMILES)

Four congeneric series, each a fixed scaffold + an N-alkyl chain of varying length:

1. **Methoxyphenyl amide** — scaffold `COc1ccc(cc1)C(=O)N` (psa 38.33)
   - m15 (n=2), m24 (n=3), m28 (n=4), m17 (n=5), m22 (n=6), **m05 (n=11, target, unsynthesized)**
   - This is the **target's series**. All short-chain; m05's n=11 is well beyond the measured range → extrapolation.
2. **Nitrophenyl amide** — scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24)
   - m09(n=2), m12(n=3? see below), m02(n=3), m32(n=5), m33(n=6), m29(n=7), m31(n=8), m06(n=9), m10(n=10)...
3. **Sugar amide** — scaffold `OCC(O)C(O)C(O)C(=O)N` (psa 110.02)
   - m14(n=2), m27(n=3), m01(n=4), m19(n=5), m03(n=6), m21(n=7), m18(n=8), m07(n=9), m25(n=10)...
4. **Biphenyl amide** — scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.1)
   - m13, m11, m04, m30, m23, m20, m26, m08, m16 ... (psa 29.1 < 40, fails Job-2 psa gate)

Congeners of m05 (same scaffold): **m15, m24, m28, m17, m22** (and m05 itself).

## Job 1 — predict QED of m05 (chain n=11)

Measured the 5 synthesizable congeners (5 of 12 budget):

| chain n | mol_id | QED    |
|--------:|--------|--------|
| 2       | m15    | 0.7621 |
| 3       | m24    | 0.7919 |
| 4       | m28    | 0.7518 |
| 5       | m17    | 0.7498 |
| 6       | m22    | 0.7377 |

Trend: rises 2→3 (peak ≈ n=3), then declines 3→4→5→6 — a downward parabola, NOT a straight line.

`fit_trend` quadratic through the 5 points, evaluated at n=11:
- coefficients (low→high): [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188
- **prediction at n=11: 0.5416**

For comparison, a degree-1 fit predicts 0.695 (RMSE 0.01308, worse fit) and ignores the observed curvature. Per the task ("do not assume the relationship is a straight line"), I keep the **quadratic → target_qed = 0.5416**. The longer chain pulls MW up (m05 mw 305) and PSA stays low, but QED's lipophilicity/rotatable-bond penalties drive it down past the short-chain plateau; the quadratic extrapolates that continued decline.

## Job 2 — backup lead (QED ≥ 0.6, psa ≥ 40, NOT in m05's series)

Free pre-screen: `screen_candidates()` (psa ≥ 40) returns 18 molecules. Crucially, **every m05-series member has psa 38.33 < 40**, so any psa-passing candidate is automatically NOT in m05's series — the series constraint is satisfied for free across the whole screened set. (Biphenyl series, psa 29.1, also fails psa and is out.)

Remaining budget after Job 1: 7. Measured nitrophenyl-series candidates (chosen for moderate psa 72 and positive alogp):

| mol_id | chain n | QED    | pass ≥0.6? |
|--------|--------:|--------|:-----------|
| m09    | 2       | 0.5826 | no         |
| m12    | 3       | 0.4709 | no         |
| m02    | 3       | 0.6046 | **yes**    |
| m32    | 5       | 0.4683 | no         |
| m33    | 6       | 0.4591 | no         |

**Lead = m02**:
- measured QED = **0.6046 ≥ 0.6** ✓ (real measurement, budget unit 8)
- psa = 72.24 ≥ 40 ✓ (free, from pool.tsv / screen_candidates)
- scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N` → not in m05's series ✓ (free, via describe_structure)

All three constraints satisfied. m02 chosen as the lead. 2 measurement units remain unused.

## Final values for answer.json
- target_qed (m05, predicted): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids (pool molecules in m05's series): m15, m24, m28, m17, m22
