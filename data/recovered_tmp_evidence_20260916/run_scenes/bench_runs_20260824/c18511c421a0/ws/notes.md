# Task Notes

## Overview
- Compound collection with 33 molecules
- Need to find QED for m05 (not synthesizable, so need prediction)
- Need to find a molecule meeting specific criteria
- Budget of 17 units for both listing pages and measurements

## Task 1
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC)

## Task 2
- Find molecule with:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. Not in same congeneric series as m05

## Work Log

### Grouping Compounds by Scaffold
Found 4 groups:
1. COc1ccc(cc1)C(=O)N (includes m05 with chain length 11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N (chain lengths 2-10)
3. OCC(O)C(O)C(O)C(=O)N (chain lengths 2-10)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (chain lengths 2-10)

### Task 1: Predicting QED for m05
Measured QED values for compounds in the same series as m05:
- m15: chain_len=2, qed=0.7621
- m24: chain_len=3, qed=0.7919
- m28: chain_len=4, qed=0.7518
- m17: chain_len=5, qed=0.7498
- m22: chain_len=6, qed=0.7377

Used fit_trend to predict QED for chain length 11 (m05): 0.5416

### Task 2: Finding suitable molecule
Checked m02 from second group:
- Measured QED = 0.6046 (≥ 0.6 ✓)
- From descriptor data, psa = 72.24 (≥ 40 ✓)
- Different scaffold from m05 (✓)
- So m02 meets all three criteria

Final Answer:
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046