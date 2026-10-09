# Notes for QED Prediction Task

## Molecule Analysis
Target molecule: m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- Has a methoxyphenyl group connected to a carbonyl, connected to an amine with 11-carbon alkyl chain
- Belongs to congeneric series with same scaffold differing only in N-alkyl chain length

Congeneric series with same scaffold (methoxyphenyl-C(=O)-NH-alkyl):
- m15: COc1ccc(cc1)C(=O)NCC (2 carbon chain)
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbon chain)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbon chain)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbon chain)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbon chain)

I will measure some of these to establish a trend and then predict m05's QED.

Measured QED values for congeneric series:
- m15 (2 carbon chain): QED = 0.7621
- m24 (3 carbon chain): QED = 0.7919
- m28 (4 carbon chain): QED = 0.7518
- m17 (5 carbon chain): QED = 0.7498
- m22 (6 carbon chain): QED = 0.7377

For m05, which has 11 carbon chain, I expect the QED to continue decreasing based on the trend observed.

Task 1: For m05, since it has never been synthesized, measure_qed returns probe_unavailable. Based on the trend in the congeneric series, with longer N-alkyl chains generally leading to lower QED values, and considering that m05 has the longest chain (11 carbon atoms), I predict its QED to be lower than the observed values for shorter chains. Given the values: m15 (2 carbon): 0.7621, m24 (3 carbon): 0.7919, m28 (4 carbon): 0.7518, m17 (5 carbon): 0.7498, m22 (6 carbon): 0.7377, I estimate m05's QED to be approximately 0.68.

Task 2: I found that m02 satisfies all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. m02 has SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCC, which has a nitrobenzene group instead of methoxyphenyl, so it's not in the same congeneric series as m05 ✓

Results Summary:
- m05 predicted QED: ~0.68 (can't be directly measured)
- Best candidate for Task 2: m02 with measured QED of 0.6046