# Notes — QED prediction task

## Pool structure (from group_by_scaffold, free)
4 scaffolds, 33 molecules:

1. **methoxy-benzamide** `COc1ccc(cc1)C(=O)N` — m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target, unsynth)**
2. **nitro-benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **sugar-amide** `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **biphenyl-amide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Page 1 descriptors (cost 1)
- m01 (sugar, chain4): mw221, alogp -2.022, psa110, rotb7, arom0
- m02 (nitro, chain3): mw208, alogp 1.735, psa72.24, rotb4, arom1  ← strong Task-2 candidate
- m03 (sugar, chain6): mw249, alogp -1.242, psa110, rotb9, arom0
- m04 (biphenyl, chain4): mw253, alogp 3.884, psa29.1, rotb5, arom2  ← psa<40, fails

## Strategy
- Budget 12 total; 1 used on page 1 → 11 left.
- Task 1: measure m05's 5 synthesizable congeners (m15,m24,m28,m17,m22) → fit QED vs chain_len, eval at 11.
- Task 2: m02 (nitro series, not m05's series; psa 72.24≥40; drug-like descriptors → expect QED≥0.6). Measure it. Backups if needed: other nitro short-chain members (m09, m12) — all share psa~72.

## Measurements (budget after: 5 of 12 remaining)
Methoxy-benzamide congeners (m05's series) for Task 1:
- m15 chain2 → 0.7621
- m24 chain3 → 0.7919  (peak)
- m28 chain4 → 0.7518
- m17  chain5 → 0.7498
- m22  chain6 → 0.7377
Task-2 candidate:
- m02 (nitro, chain3) → 0.6046

## Task 1 — prediction at chain_len 11
These 5 are the *only* synthesizable members of the methoxy series (no chains 7–10 in
pool), so the m05 estimate must come from extrapolating these points.
- Quadratic (degree 2, default): 0.5416, RMSE 0.01188
- Linear (degree 1): 0.6950, RMSE 0.01308

Decision: report **0.695** (linear). Extrapolating 5 units past the data: the linear
model is the robust choice; the quadratic's slight in-sample curvature squares into an
accelerating decline at x=11 that overstates the drop. Independent sanity check on m05
(COc1ccc(cc1)C(=O)N + C11 chain): MW≈305 (QED-friendly), rotb≈12 (penalised), alogp≈6
(main penalty), psa≈40, arom=1 — a geometric-mean estimate of the desirabilities lands
~0.65–0.73, matching the linear fit and well above the quadratic's 0.54. → target_qed=0.695.

## Task 2 — lead
m02: QED 0.6046 (≥0.6 ✓), psa 72.24 (≥40 ✓), nitro scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`
≠ m05's methoxy scaffold `COc1ccc(cc1)C(=O)N` ✓. All three constraints satisfied.
→ lead_mol_id=m02, lead_qed_measured=0.6046.
