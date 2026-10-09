# Notes for QED Prediction and Lead Compound Selection

## Job 1: Predict the QED of m05

### Analysis of Congeneric Series
The target molecule m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to a series with the core structure: methoxyphenyl-C(=O)-N-alkyl chain. 

The compounds in the same series as m05 (same scaffold, different chain lengths):
- m15: COc1ccc(cc1)C(=O)NCC (chain length: 2)
- m24: COc1ccc(cc1)C(=O)NCCC (chain length: 3)
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length: 4)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length: 5)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length: 6)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length: 11)

### QED Measurements for Congeneric Series
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

### Trend Analysis and Prediction
Using a quadratic polynomial fit (degree=2) through the measured points (chain lengths 2-6), I extrapolated the QED for chain length 11 (m05):
- Predicted QED for m05: 0.5416

The trend shows that QED initially increases slightly from chain length 2 to 3, then gradually decreases as the chain gets longer. This indicates an optimal chain length around 3-4 carbon atoms for this particular scaffold.

## Job 2: Select a Lead Compound Meeting All Criteria

### Criteria
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in the same series as m05

### Screening Process
First, I used screen_candidates() to identify compounds with PSA ≥ 40. Then, I excluded compounds from the same series as m05.

### Measurement Results
- m01 (PSA: 110.02): QED = 0.3187 (< 0.6, failed)
- m02 (PSA: 72.24): QED = 0.6046 (≥ 0.6, passed all criteria!)
- m03 (PSA: 110.02): QED = 0.3282 (< 0.6, failed)

### Selected Lead Compound
m02 meets all three criteria:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC (different scaffold from m05) ✓

### Summary
- Predicted QED for m05: 0.5416
- Selected lead compound: m02
- Lead compound QED: 0.6046
- Congeners of m05: m15, m24, m28, m17, m22, m05