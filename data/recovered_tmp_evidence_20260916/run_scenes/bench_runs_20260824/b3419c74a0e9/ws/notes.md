# Task notes

## Goal
- Job 1: predict QED of `m05` (never synthesized). SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. Scaffold `COc1ccc(cc1)C(=O)N`, chain_len 11.
- Job 2: pick one lead with: measured QED ≥ 0.6, psa ≥ 40, not in m05's series.
- Budget: 17 measurements total (shared). list_pool pages also cost budget. Used so far: 1 (list_pool page 1).

## Series map (from free describe_structure on all 33)

### Series A — `COc1ccc(cc1)C(=O)N` (m05's series, THE TARGET)
- m05: c11 (target, unsynthesized → probe_unavailable)
- Pool members (short-chain only): m15(c2), m24(c3), m28(c4), m17(c5), m22(c6)
- → extrapolate from c2–c6 out to c11.

### Series B — `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide)
- m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10)

### Series C — `OCC(O)C(O)C(O)C(=O)N` (sugar amide)
- m14(c2), m27(c3), m01(c4), m19(c5), m21(c7), m18(c8), m07(c9), m25(c10)

### Series D — `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl benzamide)
- m13(c2), m11(c3), m30(c5), m23(c6), m20(c7), m26(c8), m08(c9), m16(c10)
- All psa ≈ 29.1 < 40 → excluded from lead screen.

## psa ≥ 40 candidates (free screen) — 18 pass
m01,m02,m03,m06,m07,m09,m10,m12,m14,m18,m19,m21,m25,m27,m29,m31,m32,m33
- None are Series A members (Series A's m15,m17,m22,m24,m28 all have psa<40).
- So all 18 are automatically "not in m05's series". Lead = one of these with measured QED ≥ 0.6.

## Descriptors seen (page 1 + screen rotb/psa)
- m01 (C,c4): mw221, alogp-2.02, psa110, rotb7, arom0
- m02 (B,c3): mw208, alogp1.74, psa72, rotb4, arom1  ← drug-like profile
- m03 (C,c6): mw249, alogp-1.24, psa110, rotb9, arom0
- m04 (D,c4): mw253, alogp3.88, psa29, rotb5, arom2  (psa<40, excluded)
- m09 (B,c2): rotb3, psa72 ← best-looking (low MW, good logP/PSA/rotb, arom1)
- m12 (B,c4): rotb5, psa72
- m32 (B,c5): rotb3, psa72
- m14 (C,c2): rotb5, psa110
- m27 (C,c3): rotb6, psa110

## Plan
- Job 1: measure m15,m24,m28,m17,m22 (5 meas). fit_trend degree 2 → x_query=11.
- Job 2: measure most-promising lead candidates (m09, m02, m32, m12, then sugar m14/m27 if needed) until one has QED ≥ 0.6. ~11 meas available.

## Budget tally
- Used: 1 (list_pool p1) + 5 (Job1) + 4 (Job2) = 10/17. Remaining 7 (unused).

## Job 1 — Series A QED vs chain length (target series, c2–c6 measured)
| chain | mol | QED |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 (peak) |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

m05 = same scaffold, chain 11 (unsynthesized → probe_unavailable). Pool only has short-chain members, so extrapolate beyond measured range.

Trend is clearly curved (rises to a peak at c3 then falls), so use degree-2 (NOT a straight line, per instructions).
fit_trend degree 2: coefficients (low→high) = [0.74932, 0.017024, −0.003264]
  i.e. QED(x) ≈ 0.74932 + 0.017024·x − 0.003264·x²,  RMSE 0.0119 over the 5 points.
Vertex at x ≈ 0.017024/(2·0.003264) ≈ 2.6 — matches the observed peak near c3. Good fit.
At x = 11: QED(11) = 0.74932 + 0.017024·11 − 0.003264·121 = 0.74932 + 0.18726 − 0.39494 ≈ **0.5416**.
Linear sanity check gives 0.695, but the data is visibly curved (peak then decline), so the quadratic is the right model → **target_qed = 0.5416**.

## Job 2 — lead selection
Screened 18 psa≥40 candidates; all are NOT Series A (Series A members m15,m17,m22,m24,m28 all have psa<40), so any satisfies constraint 3.
Measured best-profile candidates first:
| mol | series | chain | QED | psa | notes |
|---|---|---|---|---|---|
| m09 | B | 2 | 0.5826 | 72.24 | just below |
| m02 | B | 3 | **0.6046** | 72.24 | ✓ ≥ 0.6 |
| m32 | B | 5 | 0.4683 | 72.24 | — |
| m12 | B | 4 | 0.4709 | 72.24 | — |

**Lead = m02** (QED 0.6046 ≥ 0.6, psa 72.24 ≥ 40, Series B ≠ m05's Series A). All three constraints satisfied.

## Final answers
- target_qed = 0.5416
- lead_mol_id = "m02"
- lead_qed_measured = 0.6046
- congener_ids (Series A, m05's series, all pool members): ["m15","m24","m28","m17","m22"]
