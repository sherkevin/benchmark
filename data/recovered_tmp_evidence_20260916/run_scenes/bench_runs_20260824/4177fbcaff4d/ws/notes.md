# Notes: QED Prediction for m05

## Identifying the Congeneric Series

The target molecule m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCCCC`. This is a 4-methoxybenzamide with a 13-carbon alkyl chain on the nitrogen.

To find congeners (same scaffold, different chain length), I searched for molecules with the same `COc1ccc(cc1)C(=O)N` core structure. The following pool molecules share this scaffold:

| mol_id | SMILES | Chain | Chain Length |
|--------|--------|-------|--------------|
| m15 | COc1ccc(cc1)C(=O)NCC | ethyl | 2 |
| m24 | COc1ccc(cc1)C(=O)NCCC | propyl | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | butyl | 4 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | pentyl | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | hexyl | 6 |

Other molecules in the pool belong to different series:
- Nitrobenzamide series (O=[N+]([O-])c1ccc(cc1)C(=O)N...): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Biphenyl amide series (c1ccc(cc1)-c1ccc(cc1)C(=O)N...): m04, m08, m11, m13, m16, m20, m23, m26, m30
- Glucamide series (OCC(O)C(O)C(O)C(=O)N...): m01, m03, m07, m14, m18, m19, m21, m25, m27

## Measurements

I measured all 5 congeners in the target series (using 5 of 8 budget units):

| mol_id | Chain Length | QED (measured) |
|--------|-------------|----------------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED values show:
- Peak at n=3 (QED = 0.7919)
- Declining trend from n=3 to n=6

Fitting a linear regression to the declining portion (n=3 to n=6):
- Data points: (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
- Linear fit: QED = 0.8319 - 0.0165 × n
- R² indicates moderate fit (data is somewhat noisy)

## Extrapolation to m05 (n=13)

Using the fitted trend:
- QED(13) = 0.8319 - 0.0165 × 13
- QED(13) = 0.8319 - 0.2145
- QED(13) = 0.6174

**Predicted QED for m05: 0.617**

## Reasoning Summary

The congeneric series shows QED peaking at chain length 3, then decreasing approximately linearly as chain length increases. Extrapolating this declining trend from n=3-6 to n=13 predicts a QED of approximately 0.62. The decrease reflects reduced drug-likeness as the long alkyl chain increases lipophilicity beyond the optimal range.
