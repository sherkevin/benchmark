# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (never synthesized, probe_unavailable)
- Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in m05's series
- Budget: 12 measurements total

## m05 Analysis
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl-amide (COc1ccc(cc1)C(=O)N-)
- Alkyl chain: 11 carbons (CCCCCCCCCCC)

## Identifying Congeners (same series as m05)
Looking for molecules with same scaffold `COc1ccc(cc1)C(=O)N-` but different chain lengths:

| mol_id | SMILES | chain length | QED (measured) |
|--------|--------|--------------|----------------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 | 0.7621 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 | 0.7919 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 | 0.7518 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 | 0.7498 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 | 0.7377 |

m05 has 11 carbons - much longer than any congener in the pool!

## Trend Analysis for m05 Series
Chain length (n) vs QED:
- n=2: 0.7621
- n=3: 0.7919 (peak)
- n=4: 0.7518
- n=5: 0.7498
- n=6: 0.7377

The trend shows:
- QED peaks at n=3, then decreases
- From n=3 to n=6, the decrease is roughly linear
- Decrease per carbon: (0.7919 - 0.7377) / 3 = 0.0181 per carbon

Linear regression on n=3,4,5,6:
- Slope: -0.01646 per carbon
- Intercept: 0.83187
- QED(n) ≈ 0.83187 - 0.01646*n
- QED(11) ≈ 0.83187 - 0.01646*11 = 0.6508

**Prediction for m05 (n=11): QED ≈ 0.65**

## Other Series in the Pool
1. **Series B (nitro-phenyl-amide)**: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
   - m02, m06, m09, m10, m12, m29, m31, m32, m33
   - psa = 72.24 ≥ 40 ✓

2. **Series C (biphenyl-amide)**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
   - m04, m08, m11, m13, m16, m20, m23, m26, m30
   - psa = 29.1 < 40 ✗ (excluded)

3. **Series D (polyol-amide)**: `OCC(O)C(O)C(O)C(=O)N-`
   - m01, m03, m07, m14, m18, m19, m21, m25, m27
   - psa = 110.02 ≥ 40 ✓

## Lead Selection
Constraints: QED ≥ 0.6, psa ≥ 40, NOT in m05 series

Candidates (psa ≥ 40, not m05 series):
- Series B: m02, m06, m09, m10, m12, m29, m31, m32, m33
- Series D: m01, m03, m07, m14, m18, m19, m21, m25, m27

## Measurements Made
| mol_id | series | QED | psa | Valid lead? |
|--------|--------|-----|-----|-------------|
| m15 | m05 | 0.7621 | 38.33 | No (same series) |
| m17 | m05 | 0.7498 | 38.33 | No (same series) |
| m22 | m05 | 0.7377 | 38.33 | No (same series) |
| m24 | m05 | 0.7919 | 38.33 | No (same series) |
| m28 | m05 | 0.7518 | 38.33 | No (same series) |
| m02 | nitro | 0.6046 | 72.24 | **YES** ✓ |
| m09 | nitro | 0.5826 | 72.24 | No (QED < 0.6) |
| m01 | polyol | 0.3187 | 110.02 | No (QED < 0.6) |
| m14 | polyol | 0.3237 | 110.02 | No (QED < 0.6) |

## Final Answer
- **m05 predicted QED**: 0.65 (extrapolated from linear trend of congeners)
- **Lead compound**: m02
  - QED = 0.6046 (measured, ≥ 0.6 ✓)
  - psa = 72.24 (≥ 40 ✓)
  - Not in m05 series ✓
- **Congeners of m05**: m15, m17, m22, m24, m28

Measurements used: 9 out of 12
