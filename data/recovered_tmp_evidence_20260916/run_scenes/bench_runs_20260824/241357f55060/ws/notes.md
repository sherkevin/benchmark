# Task notes

## Setup
- 33-molecule pool, budget 12 (shared: list_pool pages + measure_qed calls).
- m05 = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, never synthesized → measure_qed returns probe_unavailable; must predict from its congeneric series.

## Scaffold grouping (from group_by_scaffold, free)
4 scaffolds:
1. **`COc1ccc(cc1)C(=O)N`** (methoxy-aryl) — m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**. ← m05's series.
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-aryl) — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10).
3. `OCC(O)C(O)C(O)C(=O)N` (polyol) — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10).
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl) — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10).

## Task 1 — predict QED of m05 (chain_len 11)
Measured the 5 synthesizable members of m05's series (budget 1–5):
| chain_len | mol_id | QED    |
|-----------|--------|--------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: roughly flat/peak ~3 then declining.
- Quadratic fit (deg 2) at x=11 → **0.5416** (rmse 0.01188 on the 5 pts)
- Linear fit (deg 1) at x=11 → 0.695 (rmse 0.01308, worse)

Decision: use quadratic. Rationale: m05 has a long alkyl chain (C11); ALogP grows ~0.39/CH2 in this series (m24 alogp 1.835 at chain 3 → m22 alogp 3.005 at chain 6, Δ0.39/CH2). At chain 11 alogp ≈ 3.005 + 5·0.39 ≈ 4.95. QED's ALogP component is peaked at 3 and drops sharply above 5, so QED declines non-linearly at long chains. Quadratic captures that curvature; linear underestimates the drop. **target_qed = 0.5416.**

## Task 2 — pick a lead (QED ≥ 0.6, psa ≥ 40, not in m05's series)
Must be from scaffold 2, 3, or 4.
Page 1 + page 6 descriptors gathered:
- m01 (polyol, chain 4): mw 221.25, alogp -2.022, psa 110.02, rotb 7, arom 0. ← very low ALogP, high PSA → strong QED candidate.
- m02 (nitro, chain 3): mw 208.22, alogp 1.735, psa 72.24, rotb 4, arom 1. ← measured QED 0.6046 (budget 8). Just clears 0.6.
- m03 (polyol, chain 6): mw 249.31, alogp -1.242, psa 110.02, rotb 9, arom 0.
- m21 (polyol, chain 7): alogp -0.852, psa 110.02, rotb 10.
- m23 (biphenyl, chain 6): psa 29.1 → fails psa≥40.
- m22/m24/m28: m05's own series → excluded.

Measured m01 = 0.3187 (budget 9) — FAILS QED≥0.6. The polyol scaffold's many H-bond donors/acceptors tank QED despite low ALogP/high PSA. So high PSA is not sufficient.

Final Task 2 pick = **m02** (nitro scaffold, chain 3). Verified against all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓ (real measurement, budget 8)
2. psa = 72.24 ≥ 40 ✓
3. scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N` ✓

Budget used: 11 (5 series measurements + 2 list pages + m02 + m01 + m12 + m09). 1 remains.

Additional Task 2 attempts (confirming m02 is the nitro-series QED peak):
- m12 (nitro, chain 4) = 0.4709 — fails (ALogP too high past sweet spot).
- m09 (nitro, chain 2) = 0.5826 — fails (just under 0.6; ALogP a bit low).
→ Nitro-series QED peaks at chain 3 (m02 = 0.6046), confirming m02 as the best valid lead.

## Final answers
- target_qed (m05, predicted via quadratic fit of its series) = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046
