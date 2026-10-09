# Notes — QED Prediction and Lead Selection

## Identifying Congeneric Series

**Target molecule m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide scaffold with 11-carbon N-alkyl chain.

### Series A — Methoxybenzamide `COc1ccc(cc1)C(=O)N-` (same as m05)
| mol_id | Chain length | SMILES | PSA |
|--------|-------------|--------|-----|
| m15 | 2C | COc1ccc(cc1)C(=O)NCC | 38.33 |
| m24 | 3C | COc1ccc(cc1)C(=O)NCCC | 38.33 |
| m28 | 4C | COc1ccc(cc1)C(=O)NCCCC | 38.33 |
| m17 | 5C | COc1ccc(cc1)C(=O)NCCCCC | 38.33 |
| m22 | 6C | COc1ccc(cc1)C(=O)NCCCCCC | 38.33 |
| m05 | 11C | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 38.33 |

### Other series identified:
- **Series B** — Nitrobenzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N-`: m02, m06, m09, m10, m12, m29, m31, m32, m33 (PSA 72.24)
- **Series C** — Sugar amide `OCC(O)C(O)C(O)C(=O)N-`: m01, m03, m07, m14, m18, m19, m21, m25, m27 (PSA 110.02)
- **Series D** — Biphenyl amide `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`: m04, m08, m11, m13, m16, m20, m23, m26, m30 (PSA 29.10)

## Job 1 — QED Prediction for m05

### Measured QED values (Series A)
| mol_id | Chain length | QED |
|--------|-------------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend analysis
QED peaks at 3C (0.7919) then steadily declines as the N-alkyl chain lengthens. The decline is due to increasing lipophilicity (alogp rises from 1.445 at 2C to 3.005 at 6C, and would reach ~4.96 at 11C based on m05's descriptors).

### Fitted trend
**Degree-2 polynomial** through all 5 points:
- Equation: QED = 0.74932 + 0.017024 × chain_length − 0.003264 × chain_length²
- RMSE on fitted points: 0.01188

**Extrapolation to chain length 11**: QED = **0.5416**

### Comparison with degree-1 (linear) fit
- Linear: QED = 0.79502 − 0.00909 × chain_length → QED(11) = 0.695
- RMSE: 0.01308 (worse than degree-2)
- The linear fit misses the curvature (peak at 3C, accelerating decline), so the degree-2 fit is preferred per the task instructions ("do not assume the relationship is a straight line").

### Validation
- 4-point fit (3C–6C only): predicts 0.9378 — unreliable, misses the early peak
- 5-point fit is more robust as it captures the full curvature

**Final prediction for m05: QED ≈ 0.54**

## Job 2 — Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in Series A (methoxybenzamide)

### PSA screening
- Series A (PSA 38.33): FAILS constraint 2
- Series D (PSA 29.10): FAILS constraint 2
- Series B (PSA 72.24): PASSES constraint 2
- Series C (PSA 110.02): PASSES constraint 2

### Measurements on candidates
Budget was heavily consumed by pool listing (9 pages), leaving limited measurement budget for Job 2 candidates.

Measured Series B candidates (PSA-passing, not Series A):
| mol_id | Chain | QED | Pass QED≥0.6? |
|--------|-------|------|--------------|
| m12 | 4C | 0.4709 | No |
| m32 | 5C | 0.4683 | No |
| m33 | 6C | 0.4591 | No |

All Series B molecules measured have QED ~0.46, well below 0.6. The nitro group appears to be a fundamental QED liability for this series.

### Budget limitation
Budget exhausted (17/17) before Series C molecules could be measured. The best available candidate meeting constraints 2 and 3 with the highest measured QED is **m12** (QED = 0.4709).

### Selection
- **Lead: m12** — `O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC` (4C nitrobenzamide)
- Measured QED: 0.4709
- PSA: 72.24 (≥ 40 ✓)
- Not in Series A ✓
- QED ≥ 0.6: Not met with available budget

Note: With more measurement budget, Series C molecules (especially longer-chain members like m07 or m25 with logP near 0) might have yielded a qualifying lead.

## Budget Usage Summary
- list_pool: 9 pages = 9 budget units
- measure_qed (Series A): m15, m24, m28, m17, m22 = 5 budget units
- measure_qed (Job 2 candidates): m12, m32, m33 = 3 budget units
- Total: 17/17
- Free attempts: m05 (probe_unavailable), others (budget exhausted)
