# Notes: predict QED of m05, pick backup lead

## Series analysis

Target `m05`: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 4-methoxybenzoyl N-alkyl amide.

The scaffold is `COc1ccc(cc1)C(=O)N-` with an alkyl chain. Same scaffold members:
- m15: `COc1ccc(cc1)C(=O)NCC` — chain C2
- m24: `COc1ccc(cc1)C(=O)NCCC` — chain C3
- m28: `COc1ccc(cc1)C(=O)NCCCC` — chain C4
- m17: `COc1ccc(cc1)C(=O)NCCCCC` — chain C5
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` — chain C6
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — chain C11 (the target, unsynthesized)

So the same-series congeners in the pool are: m15, m24, m28, m17, m22, m05.

(Chain length = number of C after N. m15 NCC=2, m24 NCCC=3, m28 NCCCC=4, m17 NCCCCC=5, m22 NCCCCCC=6, m05 NCCCCCCCCCCC=11.)

The pool only has short-chain members (C2–C6). m05 is C11, far beyond the measured range → extrapolation.

## Plan

Budget 12 measurements, shared.

Job 1: measure the 5 synthesized congeners of m05's series (m15, m24, m28, m17, m22) — 5 measurements. Fit trend (degree 2, since not linear), extrapolate to chain length 11.

Job 2: Use screen_candidates() to get psa≥40 candidates (free). Exclude m05's series (free). Then measure until one has QED≥0.6. Remaining budget = 12-5 = 7.

## Measurements

### Job 1 — m05 series (chain length = #C after N)
| mol | chain | QED |
|-----|-------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

Trend: QED peaks around C3 (0.79) then declines as chain grows. Not linear.

Fits to chain length 11 (target m05):
- degree 2 (quadratic): prediction 0.5416, coefficients [0.74932, 0.017024, -0.003264], RMSE 0.01188
- degree 1 (linear, for comparison): prediction 0.695, RMSE 0.01308

Quadratic has lower RMSE on the measured points and matches the observed non-monotone shape (peak then decline), so it's the better model. The downward curvature (-0.003264·x²) dominates at long chains, pulling the C11 extrapolation to ~0.54. This is extrapolation well beyond the C2–C6 measured range, so it carries real uncertainty, but the quadratic is the most defensible fit of the trend actually observed.

→ target_qed = 0.5416

### Job 2 — lead

psa≥40 candidates (screen_candidates), 18 pass. None of m05's series (psa 38.33) is among them, so series exclusion is automatic here. Measured, picking low-rotb / simple nitro- and polyol-amides first:
| mol | psa | rotb | series | QED |
|-----|-----|------|--------|------|
| m02 | 72.24 | 4 | nitrobenzoyl (C3) | 0.6046 ✓ |
| m27 | 110.02 | 6 | polyol-amide (C3) | 0.333 ✗ |
| m01 | 110.02 | 7 | polyol-amide (C4) | 0.3187 ✗ |

m02 passes all three constraints:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (4-nitrobenzoyl) ≠ m05's `COc1ccc(cc1)C(=O)N-` (4-methoxybenzoyl) ✓

→ lead_mol_id = m02, lead_qed_measured = 0.6046

Total measurements used: 8 of 12.

## congener_ids (m05's series, all in pool)
m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6), m05 (C11).
