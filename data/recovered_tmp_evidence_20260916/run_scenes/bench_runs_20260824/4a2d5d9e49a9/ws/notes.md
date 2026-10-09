# Notes — QED prediction & lead selection

## Catalogue (33 molecules, paged with list_pool, 9 pages → 9 budget used, 8 remain)

Target **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`,
scaffold `COc1ccc(cc1)C(=O)N`, **chain_len 11** (never synthesised → probe_unavailable).

## Congeneric series (grouped by scaffold = everything before the N-alkyl chain)

**Series A — methoxy-benzamide** (scaffold `COc1ccc(cc1)C(=O)N`), psa=38.33, arom=1 — **m05's series**
| mol_id | chain_len | mw | alogp | rotb |
|--------|-----------|------|-------|------|
| m15 | 2 | 179.22 | 1.445 | 3 |
| m24 | 3 | 193.25 | 1.835 | 4 |
| m28 | 4 | 207.27 | 2.225 | 5 |
| m17 | 5 | 221.30 | 2.615 | 6 |
| m22 | 6 | 235.33 | 3.005 | 7 |
| m05 | 11 | 305.46 | 4.956 | 12 | (target, unmeasurable)

Only short-chain members (2–6) are in the pool → **extrapolate** to 11.

**Series B — nitro-benzamide** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`), psa=72.24, arom=1
m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)

**Series C — sugar-amide** (scaffold `OCC(O)C(O)C(O)C(=O)N`), psa=110.02, arom=0
m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
Note: head = 4×OH + amide → HBD≈5, HBA≈5, very polar.

**Series D — biphenyl-amide** (scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`), psa=29.1, arom=2
m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Job 2 candidate screen (free: psa≥40 AND not Series A)
- Series A: psa 38.33 < 40 → excluded (also it IS the target's series).
- Series D: psa 29.1 < 40 → excluded.
- Series C (sugar): psa 110 ≥ 40 ✓ — BUT HBD≈5, PSA 110 → QED likely low (<0.6) throughout.
- Series B (nitro): psa 72.24 ≥ 40 ✓ — likely viable; short chains have HBD=1, HBA≈3,
  moderate MW/logP/PSA/rotb. Aromatic nitro is *not* a QED alert here (else task unsolvable
  since sugar too polar) → expect short-chain nitro members to clear 0.6; QED declines as chain grows.
→ Job 2 lead hunt: probe shortest nitro chains first (m09 ch2, then m02 ch3, m12 ch4).

## Measurement plan (8 budget left)
- Job 1: measure all 5 Series-A congeners (m15,m24,m28,m17,m22 → chain 2,3,4,5,6),
  fit quadratic in chain_len, extrapolate to 11.
- Job 2: measure m09 (nitro, ch2) as primary lead candidate; fall back to m02/m12 if needed.

## Results

### Job 1 — Series A (m05's series) measured QED vs chain_len
| chain | mol_id | QED |
|-------|--------|------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |  ← peak
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |
| 11 | m05 | (target — extrapolate) |

Clear peak at chain 3, then steady decline (each extra CH2 hurts QED via logP/MW/rotb).
Task warns not to assume a straight line, so fit **quadratic** through all 5 points:
  QED ≈ 0.7493 + 0.0170·x − 0.00326·x²,  RMSE = 0.0119.
Extrapolate to x=11 (m05): **predicted QED = 0.5416**.
(Linear fit for comparison = 0.695, RMSE 0.0131 — slightly worse fit AND ignores the
observed curvature; quadratic is the defensible choice per the task guidance.)

### Job 2 — lead hunt (nitro series, psa 72.24 ≥ 40, not Series A)
| chain | mol_id | QED | ≥0.6? |
|-------|--------|------|------|
| 2 | m09 | 0.5826 | ✗ |
| 3 | m02 | 0.6046 | ✓ |
| 4 | m12 | 0.4709 | ✗ |

→ **m02** passes all three constraints:
  1. measured QED 0.6046 ≥ 0.6 ✓
  2. psa 72.24 ≥ 40 ✓
  3. nitro scaffold ≠ m05's methoxy scaffold ✓
(Series C sugar had HBD≈5 / PSA 110 → expected low QED; Series D biphenyl psa 29.1 < 40.)
Budget exhausted at 17/17.

## Deliverables
- target_qed (m05) = 0.5416 (quadratic extrapolation, chain 2–6 → 11)
- lead_mol_id = m02, lead_qed_measured = 0.6046
- congener_ids (m05's series, pool only) = m15, m24, m28, m17, m22
