# Notes — QED prediction & lead selection

## Budget
12 measurements total, shared by both jobs. Used 9 (5 for Job 1, 4 for Job 2).

## Congeneric series (from SMILES / describe_structure)
Four scaffolds in the pool:

1. **m05 series** — scaffold `COc1ccc(cc1)C(=O)N` (p-methoxybenzamide), psa = 38.33
   - m15 chain 2, m24 chain 3, m28 chain 4, m17 chain 5, m22 chain 6, **m05 chain 11 (target, unsynthesized)**
   - Pool only holds short chains (2–6); target is chain 11 → EXTRAPOLATION beyond measured range.
   - psa 38.33 < 40 → these all fail the lead's psa constraint, so none can be the lead anyway.

2. **nitro series** — scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`, psa = 72.24
   - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)

3. **sugar series** — scaffold `OCC(O)C(O)C(O)C(=O)N`, psa = 110.02
   - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)

4. **biphenyl series** — scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`, psa = 29.1 (< 40, fails psa screen)
   - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Job 1 — predict QED of m05 (chain 11, target)

Measured the m05 series (chains 2–6):

| mol_id | chain | QED     |
|--------|-------|---------|
| m15    | 2     | 0.7621  |
| m24    | 3     | 0.7919  |
| m28    | 4     | 0.7518  |
| m17    | 5     | 0.7498  |
| m22    | 6     | 0.7377  |

Trend: rises from c2 to c3 then falls through c6 — a clear curve, NOT a line. So a
degree-2 polynomial is the right model (the task warns against assuming a straight line).
fit_trend degree=2 on (chain_len, QED), x_query=11 → coefficients [c0,c1,c2] =
[0.74932, 0.017024, −0.003264], RMSE 0.0119 over the 5 points.

Extrapolating to chain 11: **QED ≈ 0.5416** (downward curvature accelerates beyond the
measured range). The linear fit (0.695) ignores the observed curvature and is rejected.
This direction is physically consistent: m05 has high alogp 4.956, MW 305, rotb 12 — all
drag QED down at long chain length.

target_qed = 0.5416

## Job 2 — pick a lead (measured QED ≥ 0.6, psa ≥ 40, not m05 series)

psa ≥ 40 screen passes 18 molecules (nitro 72.24 + sugar 110.02 series; biphenyl 29.1
and m05 series 38.33 both fail). All 18 pass the "not m05 series" constraint, so the
binding gate is measured QED ≥ 0.6.

Strategy: QED in each series is highest at short chain length, so measure the short-chain
members of the nitro and sugar series first (best logp / fewest rotatable bonds).

Measurements:
- m09 (nitro, c2): 0.5826 — just below 0.6
- m02 (nitro, c3): **0.6046 — ≥ 0.6, all constraints met** ✓
- m12 (nitro, c4): 0.4709 — below
- m14 (sugar, c2): 0.3237 — well below

**Chosen lead: m02.** Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's
`COc1ccc(cc1)C(=O)N` (different series ✓), psa 72.24 ≥ 40 ✓, measured QED 0.6046 ≥ 0.6 ✓.

lead_mol_id = m02, lead_qed_measured = 0.6046.

## congener_ids (m05's own series)
m15, m24, m28, m17, m22 (pool members; m05 itself is the target, not a pool congener).
