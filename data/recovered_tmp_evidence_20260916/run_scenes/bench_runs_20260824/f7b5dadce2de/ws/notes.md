# Notes

## Task overview
- 33-molecule pool, budget = 17 units shared between `list_pool` (1/page) and `measure_qed` (1/measurement).
- Task 1: predict QED of `m05` (scaffold `COc1ccc(cc1)C(=O)N`, chain_len 11). Never synthesized -> extrapolate from same-series measurements.
- Task 2: pick one molecule with measured QED >= 0.6, psa >= 40, NOT in m05's series.

## Scaffold grouping (free from group_by_scaffold)
4 scaffolds:
1. `COc1ccc(cc1)C(=O)N` (methoxybenzamide) = **m05's series (EXCLUDED for Task 2)**
   - m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, target)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide)
   - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` (sugar-like, multiple OH) -> likely HIGH psa
   - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl carboxamide) -> likely LOW psa
   - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Strategy
- Task 1: measure QED of the 5 available m05-series members (chains 2-6), fit degree-2 polynomial, extrapolate to chain 11.
- Task 2: sugar series (scaffold 3) likely has high psa (multiple OH). Short-chain members (m14, m27) likely have decent QED. Need psa from list_pool + measured QED.

## Task 1 — m05 series measurements (scaffold 1, extrapolate to chain 11)
| chain | mol_id | QED |
|------|--------|------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

fit_trend to x=11:
- degree 2: prediction 0.5416, rmse 0.01188, coeffs [0.74932, 0.017024, -0.003264]
- degree 1: prediction 0.695, rmse 0.01308

Degree-2 has lower RMSE and captures the real peak at chain 3 + accelerating
decline (chemically sensible: long alkyl chain hurts drug-likeness). Use 0.5416
as target_qed for m05.

## Budget tracking
- First measure batch (m15,m24,m28,m17,m22) errored ("internal error") but still
  consumed 5 units (m15 came back as measurements_used=6).
- Second batch: m15=6, m24=7, m28=8, m17=9, m22=10.
- Used = 10, remaining = 7.

## Task 2 — psa reasoning (no need to spend budget listing)
- Scaffold 3 (sugar, 4×OH + amide): psa ≈ 123 → safely >= 40 for ALL members.
- Scaffold 2 (nitro + amide): psa ≈ 88 → safely >= 40 for ALL members.
- Scaffold 4 (biphenyl + amide): psa ≈ 43 → marginal, avoid.
So pick from scaffold 2 or 3. Constraint reduces to measured QED >= 0.6.
Short chains tend to have higher QED; measure chain-2/3 members.

## Task 2 — candidate measurements
| mol_id | series | chain | QED | meets QED>=0.6? |
|--------|--------|-------|------|------|
| m09 | nitro | 2 | 0.5826 | no |
| m14 | sugar | 2 | 0.3237 | no |
| m02 | nitro | 3 | 0.6046 | **YES** |
| m27 | sugar | 3 | 0.333 | no |

## Task 2 — decision
PICK m02:
- measured QED = 0.6046 >= 0.6 ✓
- scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide): nitro (2 O) + amide
  (C=O + N-H) → psa ≈ 88 >= 40 ✓
- not in m05's series (m05 = methoxybenzamide scaffold `COc1ccc(cc1)C(=O)N`) ✓
All three constraints satisfied. Budget used = 14/17.

## Verification (list_pool page 1, cost 1)
m02 descriptors (from catalogue): mw 208.22, alogp 1.735, **psa 72.24**,
rotb 4, arom 1. psa = 72.24 >= 40 ✓ (constraint 2 verified directly, not just estimated).
Budget after verification = 15/17. m02 fully satisfies all three constraints.

## Final answers
- target_qed (m05, chain 11, degree-2 extrapolation) = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046
