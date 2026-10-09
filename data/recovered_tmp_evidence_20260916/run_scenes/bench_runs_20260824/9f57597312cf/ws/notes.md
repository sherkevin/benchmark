# Notes

## Task 1: Predict QED for m05

m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain length 11.

Measured QEDs for congeners in the same series:
- m15 (chain_len=2): 0.7621
- m24 (chain_len=3): 0.7919
- m28 (chain_len=4): 0.7518
- m17 (chain_len=5): 0.7498
- m22 (chain_len=6): 0.7377

Used polynomial fit (degree=2) to predict QED at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Find lead molecule

Constraints:
1. QED ≥ 0.6
2. psa ≥ 40
3. Not in m05's congeneric series (scaffold `COc1ccc(cc1)C(=O)N`)

Analyzed scaffolds by psa:
- `COc1ccc(cc1)C(=O)N` (m05's series): psa=38.33 - excluded (also same series)
- `O=[N+]([O-])c1ccc(cc1)C(=O)N`: psa=72.24 - valid candidates
- `OCC(O)C(O)C(O)C(=O)N`: psa=110.02 - valid candidates
- `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: psa=29.1 - excluded (psa < 40)

Measured QEDs for candidates:
- m02: QED=0.6046, psa=72.24, scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` - **PASSES ALL**
- m01: QED=0.3187 - fails (QED < 0.6)
- m14: QED=0.3237 - fails (QED < 0.6)

**Selected lead: m02**
- QED measured: 0.6046
- psa: 72.24 (≥ 40)
- Different scaffold from m05

## Budget Summary
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units used
