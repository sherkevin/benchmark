# Notes — QED Prediction and Lead Selection

## Identifying the Congeneric Series for m05

**m05** SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: `COc1ccc(cc1)C(=O)N` (methoxyphenyl amide) with N-alkyl chain of 11 carbons.

Pool molecules sharing this scaffold (same scaffold, only chain length differs):

| mol_id | SMILES suffix (N-chain) | Chain length | QED (measured) |
|--------|-------------------------|-------------|----------------|
| m15    | NCC                     | 2           | 0.7621         |
| m24    | NCCC                    | 3           | 0.7919         |
| m28    | NCCCC                   | 4           | 0.7518         |
| m17    | NCCCCC                  | 5           | 0.7498         |
| m22    | NCCCCCC                 | 6           | 0.7377         |

Congener IDs: **m15, m24, m28, m17, m22**

## Trend Fitting for m05 (Job 1)

The data shows a clear non-linear trend: QED rises from chain 2 to 3 (peak at 0.7919), then
declines steadily through chains 4, 5, 6. This concave-down shape calls for a quadratic fit.

**Quadratic fit (degree 2):**
- Coefficients: y = 0.74932 + 0.017024*x - 0.003264*x^2
- RMSE on training points: 0.01188
- Prediction at x=11 (m05): **0.5416**

**Linear fit (degree 1, for comparison):**
- Coefficients: y = 0.79502 - 0.00909*x
- RMSE: 0.01308
- Prediction at x=11: 0.6950

The linear fit misses the initial rise (chain 2 to 3) and underestimates the curvature.
The quadratic captures the observed peak and subsequent decline, which is chemically
sensible (increasing lipophilicity from longer chains eventually hurts QED). I use the
quadratic prediction: **target QED = 0.5416**.

## Lead Selection (Job 2)

### Free screens first (no measurements spent):
- **PSA >= 40**: Screened using screen_candidates(). 18 molecules pass.
  - Series B (nitrophenyl, psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
  - Series C (sugar-like, psa=110.02): m14, m27, m01, m19, m03, m21, m07, m18, m25
  - Series D (biphenyl, psa=29.1): ALL FAIL PSA, eliminated entirely
- **Not in m05's series**: Excludes m15, m24, m28, m17, m22 (already not in PSA-passing list)

### Measurements on candidates (7 remaining budget after 5 for Job 1):
| mol_id | Series | QED    | Pass all 3? |
|--------|--------|--------|-------------|
| m09    | B      | 0.5826 | No (QED < 0.6) |
| m02    | B      | 0.6046 | **YES** |
| m12    | B      | 0.4709 | No |
| m32    | B      | 0.4683 | No |
| m01    | C      | 0.3187 | No |
| m33    | B      | 0.4591 | No |
| m03    | C      | 0.3282 | No |

### Chosen lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` (nitrophenyl amide, 3-carbon chain)
- Measured QED: **0.6046** >= 0.6 (constraint 1)
- PSA: **72.24** >= 40 (constraint 2)
- Not in m05's series: Series B (nitrophenyl) != Series A (methoxyphenyl) (constraint 3)

m02 is the only molecule measured that meets all three constraints simultaneously.
Total measurements used: 12/12.
