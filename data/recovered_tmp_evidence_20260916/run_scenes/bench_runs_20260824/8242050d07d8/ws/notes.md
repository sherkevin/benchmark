# Notes: QED Prediction and Lead Selection

## Budget Allocation
- list_pool: 9 pages × 1 = 9 budget units (pages 1–9 to see all 33 molecules)
- measure_qed: 8 measurements (5 for m05 series + 3 for lead candidates)
- Total: 17/17 budget used

## Job 1: Predict QED of m05

### m05 Structure
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxyphenyl amide)
- Chain length: 11

### m05 Series Congeners (same scaffold, different chain lengths)
| mol_id | chain_len | SMILES | QED (measured) |
|--------|-----------|--------|----------------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC | 0.7621 |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC | 0.7919 |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC | 0.7518 |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC | 0.7498 |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC | 0.7377 |

congener_ids: ["m15", "m24", "m28", "m17", "m22"]

### Observed Trend
QED rises from C2 (0.7621) to a peak at C3 (0.7919), then steadily declines:
C3→C4: −0.040, C4→C5: −0.002, C5→C6: −0.012.
The non-monotonic behavior (peak then decline) indicates curvature — a straight line is inappropriate.

### Fitted Trend (degree-2 polynomial, all 5 points)
QED(x) = 0.74932 + 0.017024·x − 0.003264·x²
RMSE on fitted points: 0.01188

### Extrapolation to chain_len = 11
QED(11) = 0.74932 + 0.017024·11 − 0.003264·121 = 0.5416

For comparison, a linear fit gives 0.695 (RMSE 0.01308), but the task explicitly says
"Do not assume the relationship is a straight line" and the quadratic has lower RMSE.
The downward curvature makes physical sense: m05 has alogp = 4.956, well above the
QED-optimal range, so QED should be substantially lower than the short-chain members.

**Predicted QED for m05: 0.5416**

## Job 2: Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05 series (scaffold ≠ COc1ccc(cc1)C(=O)N)

### Pre-screening (free)
- m05 series congeners all have PSA = 38.33 → fail constraint 2 regardless.
- screen_candidates() returned 18 molecules with PSA ≥ 40 (sugar and nitrophenyl series).
- Biphenyl series (PSA = 29.1) all fail constraint 2.

### Measurements on candidates
| mol_id | series | chain | PSA | QED |
|--------|--------|-------|-----|-----|
| m12 | nitrophenyl | 4 | 72.24 | 0.4709 |
| m32 | nitrophenyl | 5 | 72.24 | 0.4683 |
| m33 | nitrophenyl | 6 | 72.24 | 0.4591 |

All three nitrophenyl candidates have QED ~0.46–0.47, below the 0.6 threshold.
Budget exhausted before sugar-series candidates (PSA=110) could be measured.

### Retrospective
With 9/17 budget spent on list_pool pages, only 8 measurements were available.
5 were needed for the m05 series trend, leaving only 3 for lead screening — insufficient
to explore both nitrophenyl and sugar series. Fewer pages (stopping at page 5–6) would
have freed 3–4 extra measurements for broader lead screening.

### Lead Selection
Best available measured candidate satisfying constraints 2 (PSA≥40) and 3 (not m05 series):
**m12** — measured QED = 0.4709, PSA = 72.24, nitrophenyl series.
Note: QED does not reach the 0.6 threshold; this is the highest-QED non-m05-series
candidate with PSA ≥ 40 that was measured before budget exhaustion.
