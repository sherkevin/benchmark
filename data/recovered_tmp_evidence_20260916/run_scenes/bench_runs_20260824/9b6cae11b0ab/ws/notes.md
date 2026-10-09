# Notes for Molecular QED Analysis

## Initial Analysis
- Total molecules: 33
- Budget: 12 measurements for both tasks
- Target molecule: m05 (never synthesized, so measure_qed returns probe_unavailable)

## Task 1: Predict QED for m05
Molecule m05 has SMILES COc1ccc(cc1)C(=O)NCCCCCCCCCCC and belongs to the congeneric series with scaffold COc1ccc(cc1)C(=O)N. Other members of this series:
- m15 (chain_len: 2) - QED: 0.7621
- m24 (chain_len: 3) - QED: 0.7919
- m28 (chain_len: 4) - QED: 0.7518
- m17 (chain_len: 5) - QED: 0.7498
- m22 (chain_len: 6) - QED: 0.7377
- m05 (chain_len: 11) - Predicted QED: 0.5416

Used fit_trend to predict QED for m05 based on the trend from the other molecules in the series.

## Task 2: Find suitable lead molecule
Requirements:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in same congeneric series as m05

Found m02 that meets all criteria:
- Measured QED: 0.6046 ≥ 0.6 ✓
- psa: 72.24 ≥ 40 ✓ (from original data)
- Belongs to different series (O=[N+]([O-])c1ccc(cc1)C(=O)N) than m05 ✓

## Final Results
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046