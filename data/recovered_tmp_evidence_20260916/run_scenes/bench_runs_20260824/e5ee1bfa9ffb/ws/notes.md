# Reasoning Notes

## Job 1: Predict the QED of m05

### Identifying Congeners
Molecule m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has a methoxyphenyl group attached to a carbonyl nitrogen with an 11-carbon chain. I identified the following molecules in the same congeneric series (same scaffold, only chain length differs):

- m15: COc1ccc(cc1)C(=O)NCC (2 carbon chain after N)
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbon chain after N)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbon chain after N)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbon chain after N)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbon chain after N)

### Measuring QED Values
- m15 (2 carbons): QED = 0.7621
- m24 (3 carbons): QED = 0.7919
- m28 (4 carbons): QED = 0.7518
- m17 (5 carbons): QED = 0.7498
- m22 (6 carbons): QED = 0.7377

### Trend Analysis and Prediction
I fitted a quadratic polynomial (degree 2) through these 5 data points to predict the QED for m05 which has 11 carbon atoms in the chain. The fitted curve showed a general decreasing trend in QED as the chain length increases beyond 3-4 carbons.

Using fit_trend with xs=[2, 3, 4, 5, 6], ys=[0.7621, 0.7919, 0.7518, 0.7498, 0.7377], and x_query=11, the predicted QED for m05 is 0.5416.

## Job 2: Pick a Backup Lead Compound

### Screening Process
I first used screen_candidates() to identify molecules with PSA ≥ 40. Then I eliminated those that belonged to the same congeneric series as m05 (those with the methoxyphenyl scaffold).

### Final Lead Selection
I tested m02 which has:
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- PSA: 72.24 ≥ 40 ✓
- Not in m05's series ✓ (nitrobenzene vs methoxyphenyl)
- Measured QED: 0.6046 ≥ 0.6 ✓

m02 meets all three required constraints and was selected as the lead compound.