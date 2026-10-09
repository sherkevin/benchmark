# Task notes

## Setup
- Budget: 12 measurements total, shared by Task 1 and Task 2.
- m05 (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) is never synthesized → must predict its QED
  from its OWN congeneric series (same scaffold, varying N-alkyl chain length).

## Scaffolds (from group_by_scaffold)
4 scaffolds:
1. `COc1ccc(cc1)C(=O)N` (methoxy-benzamide) — **m05's series**
   - m15 cl2, m24 cl3, m28 cl4, m17 cl5, m22 cl6, **m05 cl11 (target, not synthesizable)**
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide) — psa 72.24
   - m09 cl2, m02 cl3, m12 cl4, m32 cl5, m33 cl6, m29 cl7, m31 cl8, m06 cl9, m10 cl10
3. `OCC(O)C(O)C(O)C(=O)N` (sugar) — psa 110.02
   - m14 cl2, m27 cl3, m01 cl4, m19 cl5, m03 cl6, m21 cl7, m18 cl8, m07 cl9, m25 cl10
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl-benzamide) — psa 29.1 (fails psa≥40)
   - m13 cl2, m11 cl3, m04 cl4, m30 cl5, m23 cl6, m20 cl7, m26 cl8, m08 cl9, m16 cl10

## Task 1: measure m05's series
Measurements (used 5/12):
- cl2 m15: 0.7621
- cl3 m24: 0.7919  ← peak
- cl4 m28: 0.7518
- cl5 m17: 0.7498
- cl6 m22: 0.7377

### Trend drivers (qed_components)
As chain length grows within the methoxy series:
- MW: improves (small MW scores low; bigger → better up to optimal ~300)
  m15 MW=0.33 (mw179) → m24 0.41 (mw193) → m28 0.49 (mw207) → m17 0.59 (mw221) → m22 0.68 (mw235)
- ALOGP: improves toward 1.0 (m22 = 0.997)
- ROTB: degrades (more rotatable bonds): m15 0.99 → m24 0.97 → m28 0.85 → m17 0.69 → m22 0.53
- ALERTS: degrades past cl4: m15 0.84 → m24 0.84 → m28 0.64 → m17 0.64 → m22 0.64
- PSA, HBA, HBD, AROM: constant across series (0.933, 0.93, 0.99, 0.83)

For m05 (cl11): MW=305 (closer to ideal → MW score ~0.95), ALOGP ~4.96 (still good → ~1.0),
but ROTB=12 (way past 10 → score ~0.3) and ALERTS long C11 alkyl (poor). Net: continued decline.

### Fit to predict m05 (cl=11)
- Quadratic (degree 2): prediction = **0.5416**, RMSE on points 0.01188
  coeffs (low→high): 0.74932, 0.017024, -0.003264
- Linear (degree 1): prediction = 0.695, RMSE 0.01308

Quadratic has lower RMSE and properly captures the cl3 peak + decline. The physical drivers
(ROTB and ALERTS degrading with chain length, only partially offset by MW/ALOGP gains)
support a continued, accelerating decline → quadratic is the better-grounded extrapolation.

**Task 1 answer: target_qed ≈ 0.5416** (quadratic fit of m05's own series at cl=11)

## Task 2: pick one molecule
Constraints: QED≥0.6 AND psa≥40 AND not in m05's (methoxy) scaffold.
Candidates by psa:
- biphenyl series: psa 29.1 → all fail psa≥40. EXCLUDED.
- nitro series: psa 72.24 ✓ (not m05 scaffold)
- sugar series: psa 110.02 ✓ (not m05 scaffold)

Budget after Task 1: 12-5 = 7.

### Nitro series probe (sweet spot around cl3 where methoxy peaked)
Measurements (used 3 more, total 8/12):
- cl3 m02: 0.6046 ✓ (≥0.6) — CANDIDATE
- cl4 m12: 0.4709 ✗
- cl5 m32: 0.4683 ✗

m02 (cl3, nitro) satisfies ALL THREE constraints:
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold = nitro-benzamide ≠ m05's methoxy-benzamide ✓

m02 qed_components (free): MW 0.50, ALOGP 0.92, HBA 0.98, HBD 0.99, PSA 0.93, ROTB 0.97,
AROM 0.83, ALERTS 0.24 (nitro alert — the only weak score, but other 7 are strong → 0.6046 overall).
Solid, well-balanced drug-like molecule.

**Task 2 answer: lead_mol_id = m02, lead_qed_measured = 0.6046**

## Budget used: 8/12 (m15, m24, m28, m17, m22 for Task 1; m02, m12, m32 for Task 2)
