# Notes for predicting QED of molecule m05

## Target Molecule
- Molecule ID: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Chain length: 11 carbon atoms in N-alkyl chain
- Status: Not synthesized, QED cannot be measured directly

## Search Strategy
- Searched for molecules containing the scaffold "COc1ccc(cc1)C(=O)N"
- Found 6 total molecules in the same congeneric series
- All share the same core scaffold, differing only in the length of the N-alkyl chain

## Measured Congeners
- m15: N-ethyl (2C), QED = 0.7621
- m24: N-propyl (3C), QED = 0.7919
- m28: N-butyl (4C), QED = 0.7518
- m17: N-pentyl (5C), QED = 0.7498
- m22: N-hexyl (6C), QED = 0.7377

## Trend Analysis
Looking at the QED values as a function of chain length:
- 2C: 0.7621
- 3C: 0.7919 (increases)
- 4C: 0.7518 (decreases)
- 5C: 0.7498 (slight decrease)
- 6C: 0.7377 (decrease continues)

There seems to be a maximum around 3C (N-propyl), followed by a gradual decline as the chain gets longer. This suggests a non-linear relationship where the QED peaks at medium chain length and decreases for both shorter and longer chains.

## Prediction Method
Based on the observed trend, the QED appears to peak at 3 carbon atoms (0.7919) and then decrease as the chain gets longer. With the target having 11 carbon atoms (much longer than the measured 6 carbon atoms), we can expect the QED to continue decreasing beyond the measured range.

The rate of decrease appears to be roughly linear after the peak at 3C:
- From 3C to 4C: decrease of ~0.0401
- From 4C to 5C: decrease of ~0.0020
- From 5C to 6C: decrease of ~0.0121

The average decrease per additional carbon atom from 3C onwards is approximately 0.018 per carbon. Extrapolating from the 6C value (0.7377):
- Additional 5 carbons (from 6C to 11C) ≈ 5 * 0.018 = 0.09 decrease
- Predicted QED for m05 ≈ 0.7377 - 0.09 = 0.6477

However, looking more carefully at the trend from 4C to 6C, the total decrease is 0.0141 over 2 carbon atoms, or about 0.007 per carbon. This suggests a slower rate of decline. Using this more conservative rate:
- Additional 5 carbons (from 6C to 11C) ≈ 5 * 0.007 = 0.035 decrease
- Predicted QED for m05 ≈ 0.7377 - 0.035 = 0.7027

Taking a middle approach, considering the non-linear nature of the relationship, I'll estimate a moderate decline to approximately 0.68.

Actually, looking more closely:
- From 3C (peak): 0.7919
- To 6C: 0.7377 (a drop of 0.0542 over 3 carbon atoms, or 0.018 per carbon)

From 6C to 11C is 5 more carbons, so using the same rate: 5 * 0.018 = 0.09
Predicted QED = 0.7377 - 0.09 = 0.6477

This seems reasonable given the general trend.