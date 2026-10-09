# Task Notes

## Task 1: Predicting QED for m05
- Identified that m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to the congeneric series with scaffold `COc1ccc(cc1)C(=O)N`
- Measured QED values for other molecules in the same series:
  - m15 (chain_len: 2): QED = 0.7621
  - m24 (chain_len: 3): QED = 0.7919
  - m28 (chain_len: 4): QED = 0.7518
  - m17 (chain_len: 5): QED = 0.7498
  - m22 (chain_len: 6): QED = 0.7377
- Used fit_trend to predict QED for m05 (chain_len: 11): 0.5416

## Task 2: Finding a suitable lead molecule
- Criteria: QED ≥ 0.6, psa ≥ 40, not in same series as m05
- Found that m02 meets all criteria:
  - QED = 0.6046 ≥ 0.6 ✓
  - psa = 72.24 ≥ 40 ✓
  - Belongs to different series (NO2-substituted vs methoxy-substituted) ✓
- Used all 12 measurement budget units

## Final Answer
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046