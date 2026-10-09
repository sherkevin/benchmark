# Notes for QED Prediction and Lead Compound Selection

## Analysis of Congeneric Series for m05

Molecule m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has a methoxyphenyl group connected to a carbonyl and amide nitrogen, followed by an 11-carbon alkyl chain. 

The molecules belonging to the same congeneric series (same scaffold, different chain lengths) are:
- m15: COc1ccc(cc1)C(=O)NCC (chain length: 2)
- m24: COc1ccc(cc1)C(=O)NCCC (chain length: 3)
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length: 4)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length: 5)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length: 6)

## QED Measurements for Series Members

I measured the QED values for the series members:
- m15: chain length 2, QED = 0.7621
- m24: chain length 3, QED = 0.7919
- m28: chain length 4, QED = 0.7518
- m17: chain length 5, QED = 0.7498
- m22: chain length 6, QED = 0.7377

## Trend Fitting and Prediction for m05

Using a quadratic polynomial fit (degree=2) through the measured points, I extrapolated to predict the QED for m05 with chain length 11:
- Predicted QED for m05: 0.5416

The trend shows that initially QED increases slightly from chain length 2 to 3, then gradually decreases as the chain gets longer. The prediction for chain length 11 is a significant decrease compared to the shorter chains.

## Lead Compound Selection

For the lead compound, I needed to find a molecule satisfying all three criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in the same series as m05

First, I used screen_candidates() to get molecules with PSA ≥ 40. Then I identified which of these were not in the same series as m05 by checking their scaffolds.

I then measured the QED of several candidates:
- m01: QED = 0.3187 (< 0.6) - failed
- m02: QED = 0.6046 (≥ 0.6) - passed all criteria!
- m03: QED = 0.3282 (< 0.6) - failed
- m14: QED = 0.3237 (< 0.6) - failed
- m06: QED = 0.4011 (< 0.6) - failed
- m07: QED = 0.3233 (< 0.6) - failed
- m10: QED = 0.3724 (< 0.6) - failed

Molecule m02 (SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC) met all three criteria:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Not in the same series as m05 ✓

## Summary
- Predicted QED for m05: 0.5416
- Selected lead compound: m02
- Lead compound QED: 0.6046
- Congeners of m05: m15, m24, m28, m17, m22