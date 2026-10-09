# Notes: predict m05 QED + pick lead

## Data overview (data/pool.tsv)
33 molecules. Each = benzamide-like scaffold + N-alkyl chain. Four scaffolds:
- **methoxy-benzamide** `COc1ccc(cc1)C(=O)N` (psa 38.33): m05(target), m15, m17, m22, m24, m28
- **nitro-benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24): m02,m06,m09,m10,m12,m29,m31,m32,m33
- **biphenyl-benzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.1): m04,m08,m11,m13,m16,m20,m23,m26,m30
- **glycol-amide** `OCC(O)C(O)C(O)C(=O)N` (psa 110.02): m01,m03,m07,m14,m18,m19,m21,m25,m27

## Job 1 — m05 series (methoxy-benzamide), target chain=11
Confirmed via describe_structure: m05 scaffold = `COc1ccc(cc1)C(=O)N`, chain_len 11.
Same-scaffold pool members (all SHORTER than target — extrapolation, not interpolation):
- m15 chain2, m24 chain3, m28 chain4, m17 chain5, m22 chain6

Measured (5 measurements, budget used 5/12):

| chain | mol_id | QED    |
|-------|--------|--------|
| 2     | m15    | 0.7621 |
| 3     | m24    | 0.7919 |
| 4     | m28    | 0.7518 |
| 5     | m17    | 0.7498 |
| 6     | m22    | 0.7377 |

Trend observed: rises to a peak at chain 3, then declines — clear curvature, NOT linear.

Fit quadratic through all 5 points (degree 2), evaluate at x=11:
- coefficients (low→high): 0.74932, +0.017024, -0.003264
- RMSE on given points: 0.0119
- **prediction at chain 11 = 0.5416**

Sanity check: linear fit on the declining tail (chains 3–6) gives 0.6508; quadratic is more
faithful to the observed curvature and is the one used. Task explicitly says don't assume a
straight line; the data shows a peak + decline, so the quadratic extrapolation is the defensible
choice. **target_qed = 0.5416.**

## Job 2 — lead (QED>=0.6, psa>=40, NOT methoxy series)
Free pre-checks done first:
- psa>=40 excludes methoxy (38.33) and biphenyl (29.1) series automatically.
- Remaining series both qualify on psa: nitro (72.24) and glycol (110.02).
- "Not m05's series" satisfied for ALL remaining candidates (different scaffold). The psa
  screen already removed the methoxy series, so every passing candidate is safe on constraint 3.

screen_candidates (free) returned 18 psa-passers. Strategy: prefer nitro short-chain members
(low rotb, moderate alogp — most drug-like). Measured (budget 8/12 used after these):
- m09 (nitro, chain2, rotb3): QED 0.5826 — just under 0.6, fails
- m02 (nitro, chain3, rotb4): **QED 0.6046 — PASS**
- m12 (nitro, chain4, rotb5): QED 0.4709 — fails

**Lead = m02.** Verifies all three constraints:
1. measured QED 0.6046 >= 0.6 ✓
2. psa 72.24 >= 40 ✓
3. nitro-benzamide scaffold ≠ methoxy (m05's) ✓

(4 measurements of budget remain unused — not needed; a valid lead found.)

## Final answer
- target_qed (predicted): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: [m15, m24, m28, m17, m22]  (same series as m05; m05 itself is the unsynthesized target)
