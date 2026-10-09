# Task Notes

## Job 1: Predict the QED of m05

### Analysis of Congeneric Series
Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has the scaffold COc1ccc(cc1)C(=O)N with a chain length of 11 carbon atoms on the nitrogen.

Other molecules in the same series (same scaffold, different chain lengths):
- m15: Chain length 2, SMILES: COc1ccc(cc1)C(=O)NCC
- m24: Chain length 3, SMILES: COc1ccc(cc1)C(=O)NCCC  
- m28: Chain length 4, SMILES: COc1ccc(cc1)C(=O)NCCCC
- m17: Chain length 5, SMILES: COc1ccc(cc1)C(=O)NCCCCC
- m22: Chain length 6, SMILES: COc1ccc(cc1)C(=O)NCCCCCC

### Measured QED Values for the Series
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

### Trend Analysis
Using fit_trend with polynomial degree 2 to predict the QED for chain length 11 (m05):
- xs (chain lengths): [2, 3, 4, 5, 6]
- ys (QED values): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query (target chain length): 11
- Result: Predicted QED = 0.5416

The trend shows that QED initially increases slightly from chain length 2 to 3, then gradually decreases as the chain gets longer. Extrapolating this trend to chain length 11 yields a QED of approximately 0.5416.

## Job 2: Select a Lead Compound

### Criteria for Lead Compound
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free to check from data table)
3. Not in the same series as m05 (different scaffold)

### Screening Process
First, I used screen_candidates() to get molecules with PSA ≥ 40. Then I eliminated molecules that had the same scaffold as m05 (COc1ccc(cc1)C(=O)N).

### Tested Candidates
I tested several candidates that met the PSA threshold and were not in the same series as m05:

- m01 (PSA: 110.02): Different scaffold (OCC(O)C(O)C(O)C(=O)N), QED = 0.3187 (too low)
- m02 (PSA: 72.24): Different scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N), QED = 0.6046 ✓
- m03 (PSA: 110.02): Different scaffold (OCC(O)C(O)C(O)C(=O)N), QED = 0.3282 (too low)
- m06 (PSA: 72.24): Different scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N), QED = 0.4011 (too low)
- m07 (PSA: 110.02): Different scaffold (OCC(O)C(O)C(O)C(=O)N), QED = 0.3233 (too low)
- m10 (PSA: 72.24): Different scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N), QED = 0.3724 (too low)
- m14 (PSA: 110.02): Different scaffold (OCC(O)C(O)C(O)C(=O)N), QED = 0.3237 (too low)

### Selected Lead Compound
m02 meets all three criteria:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Different scaffold than m05 ✓

### Summary
- Predicted QED for m05: 0.5416
- Selected lead compound: m02
- Measured QED of lead: 0.6046
- Congeners of m05: m15, m24, m28, m17, m22, m05