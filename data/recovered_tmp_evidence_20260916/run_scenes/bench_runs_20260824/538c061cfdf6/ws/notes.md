# Notes

## Task 1: Predict QED for m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to the scaffold `COc1ccc(cc1)C(=O)N` with chain length 11.

Members of m05's congeneric series:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11

Measured QED values for the series:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

Used fit_trend with degree=2 polynomial to predict QED at chain_len=11:
- Prediction: **0.5416**

## Task 2: Find a lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05's congeneric series

Molecules with psa ≥ 40 (not in m05's series):
- Series `OCC(O)C(O)C(O)C(=O)N`: m01, m03, m07, m14, m18, m19, m21, m25 (psa=110.02)
- Series `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)

Measured:
- m01: QED=0.3187 (fails constraint 1)
- m02: QED=0.6046 ✓ (passes all constraints)
- m14: QED=0.3237 (fails constraint 1)

**Selected: m02**
- QED: 0.6046 ≥ 0.6 ✓
- psa: 72.24 ≥ 40 ✓
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05's scaffold) ✓

## Budget Summary
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements (m15, m24, m28, m17, m22, m01, m02, m14) = 8 units
- Total: 17/17 units used
