# Notes

## Task 1: Predict QED for m05

m05 belongs to the scaffold `COc1ccc(cc1)C(=O)N` with N-alkyl chain length 11.

Measured QED for molecules in the same congeneric series:
- m15 (chain_len=2): 0.7621
- m24 (chain_len=3): 0.7919
- m28 (chain_len=4): 0.7518
- m17 (chain_len=5): 0.7498
- m22 (chain_len=6): 0.7377

Used quadratic fit (degree=2) to extrapolate to chain_len=11:
- RMSE: 0.01188
- Predicted QED for m05: **0.5416**

(Linear fit gave 0.695 with higher RMSE of 0.01308, so quadratic is preferred.)

## Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series

Candidates with psa ≥ 40:
- Nitro series (psa=72.24): m02, m09, m12, m32, m33, m29, m31, m06, m10
- Polyol series (psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

Measurements taken:
- m02: QED = 0.6046 ✓ (meets all criteria)
- m12: QED = 0.4709 (< 0.6)
- m29: QED = 0.4446 (< 0.6)
- m01: QED = 0.3187 (< 0.6)
- m19: QED = 0.3244 (< 0.6)
- m03: QED = 0.3282 (< 0.6)

**Selected: m02**
- QED = 0.6046 (≥ 0.6) ✓
- psa = 72.24 (≥ 40) ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's COc1ccc series) ✓

## Budget used
12 measurements total (5 for m05 series + 6 for Task 2 candidates + 1 extra... wait, let me recount)

Actually: 5 (m15, m24, m28, m17, m22) + 6 (m02, m12, m29, m01, m19, m03) = 11 measurements used.
