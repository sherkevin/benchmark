# Notes

## Task 1: Predict QED for m05

m05 belongs to scaffold group `COc1ccc(cc1)C(=O)N` with chain length 11.

Measured QED for other molecules in the same scaffold group:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Used fit_trend with degree=2 polynomial to predict QED at chain_len=11:
- **Predicted QED for m05 = 0.5416**

## Task 2: Find a molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's series

Scaffold groups:
1. `COc1ccc(cc1)C(=O)N` - m05's series (EXCLUDED): m15, m24, m28, m17, m22, m05 (PSA=38.33)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` - m09, m02, m12, m32, m33, m29, m31, m06, m10 (PSA=72.24)
3. `OCC(O)C(O)C(O)C(=O)N` - m14, m27, m01, m19, m03, m21, m18, m07, m25 (PSA=110.02)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` - m13, m11, m04, m30, m23, m20, m26, m08, m16 (PSA=29.1, below 40)

Group 4 excluded due to PSA < 40. Group 1 excluded as it's m05's series.

Measured QED for candidates:
- m02 (Group 2): QED = 0.6046 ✓ (≥0.6)
- m01 (Group 3): QED = 0.3187 (below 0.6)
- m09 (Group 2): QED = 0.5826 (below 0.6)

**Selected: m02**
- QED = 0.6046 (≥0.6 ✓)
- PSA = 72.24 (≥40 ✓)
- Not in m05's scaffold group ✓

## Budget Summary
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units used
