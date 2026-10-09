# Notes

## Task Overview
- Task 1: Report QED of m05 (has scaffold with N-alkyl chain, need to predict from congeneric series)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, NOT in same congeneric series as m05

## Data Summary
m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (305.46, alogp 4.956, psa 38.33, rotb 12, arom 1)
- Has a methoxy-phenyl-amide scaffold with a 13-carbon alkyl chain

## Plan
1. Group by scaffold to identify congeneric series
2. Find molecules in m05's series (same scaffold, different N-alkyl chain lengths)
3. Measure QED for several molecules in m05's series to build a trend
4. Use fit_trend to predict m05's QED based on chain length
5. For Task 2, find molecules with psa >= 40 that are NOT in m05's series
6. Measure QED for candidates until finding one with QED >= 0.6

## Execution

### m05 Series (scaffold: COc1ccc(cc1)C(=O)N)
Measured QED values:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

### Trend Fit for m05
Used fit_trend with degree=2 on chain lengths [2,3,4,5,6] and QED values above.
Prediction for chain length 11 (m05): **0.5416**

### Task 2 Candidates
Molecules with psa >= 40 and NOT in m05 series:
- Nitro series (psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Polyol series (psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

### Measurements for Task 2
- m02: QED = 0.6046 ✓ (meets all criteria)
- m09: QED = 0.5826 (below 0.6)
- m14: QED = 0.3237 (below 0.6)
- m01: QED = 0.3187 (below 0.6)

## Result
- **m05 predicted QED**: 0.5416
- **Lead molecule**: m02 (QED=0.6046, psa=72.24, not in m05 series)
- Budget used: 9 out of 12
