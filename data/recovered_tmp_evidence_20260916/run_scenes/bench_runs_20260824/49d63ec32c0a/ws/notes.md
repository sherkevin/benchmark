# Notes for QED Prediction and Lead Compound Selection

## Analysis of Congeneric Series for m05

Molecule m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to a series with the core scaffold COc1ccc(cc1)C(=O)N-R, where R varies by alkyl chain length.

Identified congeners in the same series:
- m15: COc1ccc(cc1)C(=O)NCC (chain length: 2)
- m24: COc1ccc(cc1)C(=O)NCCC (chain length: 3) 
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length: 4)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length: 5)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length: 6)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length: 11)

## Measured QED Values for Congeners

- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

## Trend Analysis

The QED values show an initial increase from chain length 2 (0.7621) to 3 (0.7919), then a gradual decrease as the chain lengthens further (4: 0.7518, 5: 0.7498, 6: 0.7377).

The rate of decrease from chain length 3 to 6 is approximately 0.018 per additional carbon atom. However, the rate appears to slow down, with decreases of -0.0401, -0.0020, and -0.0121 for each additional carbon.

Extrapolating this trend to chain length 11 (which is 5 more than our maximum measured length of 6), and accounting for the slowing rate of decrease, I predict that m05 will have a QED of approximately 0.68.

## Lead Compound Selection

Criteria for lead compound:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

After analyzing the data, I identified molecules with PSA ≥ 40 that are not in the same series as m05:
- Nitrophenyl series (O=[N+]([O-])c1ccc(cc1)C(=O)N-R): All have PSA = 72.24
- Trihydroxybutyric acid series (OCC(O)C(O)C(O)C(=O)N-R): All have PSA = 110.02

I measured several candidates and found:
- m02: QED = 0.6046, PSA = 72.24, SMILES = O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
  - Satisfies all three criteria ✓

Therefore, m02 is selected as the lead compound.