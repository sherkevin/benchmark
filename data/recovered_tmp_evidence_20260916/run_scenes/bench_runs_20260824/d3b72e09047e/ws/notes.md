Medicinal Chemistry Task Notes

Task 1: Predicting QED for m05
- Identified that m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to the congeneric series with scaffold COc1ccc(cc1)C(=O)N
- This molecule has never been synthesized, so direct measurement gives probe_unavailable
- Measured QED values for other members of the same series:
  - m15 (chain_len=2): QED = 0.7621
  - m24 (chain_len=3): QED = 0.7919
  - m28 (chain_len=4): QED = 0.7518
  - m17 (chain_len=5): QED = 0.7498
  - m22 (chain_len=6): QED = 0.7377
- Used fit_trend to predict QED for m05 (chain_len=11): 0.5416

Task 2: Finding a suitable lead molecule
- Needed a molecule with QED ≥ 0.6, PSA ≥ 40, and not in the same series as m05
- Identified molecules with PSA ≥ 40 from different scaffolds than m05
- Tested m02 which has PSA=72.24 and is in a different congeneric series
- Measured QED of m02: 0.6046, which satisfies QED ≥ 0.6
- m02 meets all three criteria: QED ≥ 0.6, PSA ≥ 40, and different series from m05