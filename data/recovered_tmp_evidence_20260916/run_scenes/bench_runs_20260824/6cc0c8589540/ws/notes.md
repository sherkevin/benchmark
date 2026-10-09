# Notes: predict m05 QED + pick backup lead

## Setup
- Budget: 12 measurements total, shared by both jobs.
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, scaffold `COc1ccc(cc1)C(=O)N`, chain_len = 11.
- m05 never synthesized → `measure_qed` returns probe_unavailable; must predict from its own series.

## Congeneric series (by scaffold, from describe_structure + SMILES)

### Series A — scaffold `COc1ccc(cc1)C(=O)N` (methoxyphenyl amide) — TARGET's series
- m15 chain2, m24 chain3, m28 chain4, m17 chain5, m22 chain6, **m05 chain11 (target)**
- Pool only has short chains (2–6). m05 is chain 11 → extrapolation beyond range.
- psa = 38.33 (< 40) for whole series, so none of these pass the lead's psa screen anyway.

### Series B — scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrophenyl amide), psa 72.24
- m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)

### Series C — scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl amide), psa 29.1
- m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)
- psa < 40 → none pass lead screen.

### Series D — scaffold `OCC(O)C(O)C(O)C(=O)N` (sugar amide), psa 110.02
- m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)

## Job 2 — lead candidates (psa ≥ 40 AND not in m05's series)
screen_candidates psa≥40 passing list (none are in series A since series A psa=38.33):
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33 (18 candidates)
All belong to series B (psa 72.24) or D (psa 110.02). Need measured QED ≥ 0.6.

## Plan
- Job 1: measure all 5 of m05's series (m15,m24,m28,m17,m22) = 5 measurements. Fit degree-2 trend, extrapolate to chain 11.
- Job 2: with remaining 7 measurements, test candidates likely to have high QED (shorter chains tend to be more drug-like). Measure until I find one ≥ 0.6.

## Job 1 results — Series A measured (m05's series)
| chain | mol | QED |
|------|-----|------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |  <- peak
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: rises to a peak at chain 3, then declines steadily. NOT a straight line (m24 > m15 > m28 despite ordering), so a line is wrong; task explicitly forbids assuming linear.

### Fits to chain 11 (extrapolation, far beyond measured 2–6)
- degree-2 (quadratic): prediction **0.5416**, RMSE 0.0119, coeffs [0.74932, 0.017024, -0.003264]
- degree-1 (line): 0.695, RMSE 0.01308 (worse fit, ignores the peak)
- degree-3: 2.1323 — impossible (>1), overfit/unstable, rejected.

→ Use quadratic, the appropriate non-linear fit with the lowest sensible RMSE.
**target_qed (m05) = 0.5416**

## Job 2 results — lead search
Used the psa≥40 screen (series B nitrophenyl psa=72.24, series D sugar psa=110.02; series A m05's methoxyphenyl psa=38.33 fails so is auto-excluded — good).
Measured short-chain members of each series:
| mol | series | chain | QED | psa | in m05 series? | ≥0.6? |
|-----|--------|-------|------|-----|----------------|-------|
| m09 | B nitro | 2 | 0.5826 | 72.24 | no | no |
| m02 | B nitro | 3 | 0.6046 | 72.24 | no | YES |
| m14 | D sugar | 2 | 0.3237 | 110.02 | no | no |
| m27 | D sugar | 3 | 0.3330 | 110.02 | no | no |

m02 wins: QED 0.6046 ≥ 0.6, psa 72.24 ≥ 40, scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N`.
Stopped early (9/12 budget) once a qualifying lead was found.

## Final answers
- target_qed = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids = [m15, m24, m28, m17, m22] (same series as m05, the measured short-chain members)

