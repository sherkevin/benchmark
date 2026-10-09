# Notes

## Task 1: Predict QED for m05

m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) cannot be measured directly (probe_unavailable).
It belongs to scaffold group `COc1ccc(cc1)C(=O)N` with members: m15, m24, m28, m17, m22, m05.

Measured QED values for the congeneric series:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Used fit_trend with degree=2 polynomial to predict QED for chain_len=11 (m05):
- **Predicted QED for m05: 0.5416**

## Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series

Scaffold groups and their psa values:
1. `COc1ccc(cc1)C(=O)N` (m05's group): psa = 38.33 (excluded)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N`: psa = 72.24 ✓
3. `OCC(O)C(O)C(O)C(=O)N`: psa = 110.02 ✓
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: psa = 29.1 (excluded)

Measured QED for candidates from scaffolds 2 and 3:
- m01 (scaffold 3, psa=110.02): QED = 0.3187 (fails QED ≥ 0.6)
- m02 (scaffold 2, psa=72.24): QED = 0.6046 ✓
- m03 (scaffold 3, psa=110.02): QED = 0.3282 (fails QED ≥ 0.6)

**Selected molecule: m02**
- QED = 0.6046 (≥ 0.6 ✓)
- psa = 72.24 (≥ 40 ✓)
- Not in m05's scaffold group ✓

## Budget usage
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements (m15, m24, m28, m17, m22, m01, m02, m03) = 8 units
- Total: 17/17 budget used
