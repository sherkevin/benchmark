# Notes

## Scaffold Groups (from group_by_scaffold)
1. **COc1ccc(cc1)C(=O)N** (m05's group): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predicting m05 QED

m05 has scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11. Measured all 5 synthesized members:

| mol_id | chain | QED |
|--------|-------|--------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

QED components for m22 (chain 6): MW=0.6836, ALOGP=0.9973, HBA=0.9305, HBD=0.9866, PSA=0.933, ROTB=0.5332, AROM=0.8273, ALERTS=0.6397

Trend fits for chain_len=11:
- Degree 2, all 5 pts: **0.5416** (RMSE=0.01188) ← chosen
- Degree 1, all 5 pts: 0.695 (RMSE=0.01308) — doesn't capture peak at chain 3
- Degree 3, all 5 pts: 2.1323 — nonsensical (QED must be 0–1), overfitting
- Degree 2, chains 3–6: 0.9378 — positive quadratic term, physically unreasonable
- Degree 1, chains 4–6: 0.7041 — local linear, doesn't capture curvature

Chose degree 2 with all 5 points because:
- Captures the peak near chain 3 (vertex at x≈2.6)
- Negative quadratic = declining QED for long chains (physically correct)
- m05 has MW=305, ALOGP=4.956, ROTB=12 — all in penalty territory
- Multiple QED components will degrade significantly vs chain 6

## Task 2: Finding a lead molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's scaffold group.

PSA analysis:
- Group 4 molecules all have PSA=29.1 → excluded (PSA < 40)
- Group 1 (m05's group) → excluded (same series)
- Group 2: PSA=72.24 ✓
- Group 3: PSA=110.02 ✓

Measurements:
| mol_id | group | PSA | QED | qualifies? |
|--------|-------|-----|---------|-----------|
| m02 | 2 | 72.24 | 0.6046 | ✅ |
| m09 | 2 | 72.24 | 0.5826 | ❌ (QED < 0.6) |
| m12 | 2 | 72.24 | 0.4709 | ❌ |
| m32 | 2 | 72.24 | 0.4683 | ❌ |
| m33 | 2 | 72.24 | 0.4591 | ❌ |
| m01 | 3 | 110.02 | 0.3187 | ❌ |
| m14 | 3 | 110.02 | 0.3237 | ❌ |
| m27 | 3 | 110.02 | 0.333 | ❌ |

Group 3 molecules all have very low QED (~0.32) due to high PSA and negative ALOGP.
Group 2 QED peaks at chain 3 (m02), matching the pattern seen in group 1.

**Best lead: m02** — QED=0.6046, PSA=72.24, group 2 (not m05's series)

## Budget Used
16 of 17 units (5 m05-series measurements + 8 other measurements + 3 pool pages)
