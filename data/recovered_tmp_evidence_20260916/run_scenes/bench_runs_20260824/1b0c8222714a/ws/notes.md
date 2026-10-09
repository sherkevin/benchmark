# Notes for QED Prediction and Lead Compound Selection

## Analysis of Congeneric Series

I identified several congeneric series in the dataset based on shared scaffolds with varying N-alkyl chain lengths:

1. Methoxyphenyl series (same as target m05): COc1ccc(cc1)C(=O)N-alkyl
   - m15: COc1ccc(cc1)C(=O)NCC (chain length 2)
   - m24: COc1ccc(cc1)C(=O)NCCC (chain length 3)
   - m28: COc1ccc(cc1)C(=O)NCCCC (chain length 4)
   - m17: COc1ccc(cc1)C(=O)NCCCCC (chain length 5)
   - m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length 6)
   - m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length 11) - target molecule

2. Nitrophenyl series: O=[N+]([O-])c1ccc(cc1)C(=O)N-alkyl
3. Diphenyl series: c1ccc(cc1)-c1ccc(cc1)C(=O)N-alkyl
4. Triol series: OCC(O)C(O)C(O)C(=O)N-alkyl

## QED Measurements for Methoxyphenyl Series

- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

## Trend Analysis and Extrapolation for m05

The QED values show an initial increase from chain length 2 to 3 (0.7621 → 0.7919), followed by a steady decline as the chain length increases further (0.7919 → 0.7518 → 0.7498 → 0.7377). This suggests an optimal chain length around 3 units for this series.

To predict the QED for m05 (chain length 11), I extrapolated the declining trend observed from chain lengths 3-6. The QED dropped from 0.7919 to 0.7377 over 3 units of chain length, which is a decrease of 0.0542. Assuming a similar rate of decline continues (though potentially slowing at longer chain lengths), I estimated that extending from chain length 6 to 11 would result in a further decrease.

Based on this analysis, I predict the QED of m05 to be approximately 0.68.

## Lead Compound Selection

For the backup lead compound, I needed one that satisfied all three constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

I first filtered molecules with PSA ≥ 40, which included many candidates. Then I excluded those in the methoxyphenyl series (m15, m24, m28, m17, m22). Finally, I measured QED values for the remaining candidates.

Among the tested molecules, m02 met all three criteria:
- QED = 0.6046 (≥ 0.6)
- PSA = 72.24 (≥ 40) 
- Part of the nitrophenyl series, not the methoxyphenyl series