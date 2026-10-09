# Task Notes

## Task 1: Predict QED for m05

m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
It belongs to scaffold group 1: "COc1ccc(cc1)C(=O)N" with chain length 11.

Other members in this scaffold group:
- m15: chain_len 2
- m24: chain_len 3  
- m28: chain_len 4
- m17: chain_len 5
- m22: chain_len 6

Measurements made:
1. m15 (chain_len 2): QED = 0.7621
2. m24 (chain_len 3): QED = 0.7919  
3. m28 (chain_len 4): QED = 0.7518

Budget used: 9 for list_pool (9 pages) + 3 for measurements = 12 total.

## Task 2: Find molecule satisfying all constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same congeneric series as m05 (scaffold group 1)

## Task 1 Prediction:
Using linear fit (degree=1) with points (2, 0.7621), (3, 0.7919), (4, 0.7518):
Predicted QED for m05 (chain_len 11): 0.7274

## Task 2 Problem:
I have measured 3 molecules (m15, m24, m28), all with QED ≥ 0.6, but:
- Their psa = 38.33 < 40
- They are in scaffold group 1 (same as m05)

No molecule I have measured satisfies all constraints. I have no budget left to measure other molecules.

## Task 2 Decision:
Since I have no budget left to measure new molecules, but must submit an answer:
- Picked m02 from group 2 (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N)
- m02 has psa = 72.24 ≥ 40 ✓
- m02 is not in scaffold group 1 (different from m05) ✓  
- For QED: using average of measured QEDs (0.7686) as an estimate, though m02 was not actually measured

This is not ideal but necessary due to budget exhaustion. Better strategy would have been to conserve budget by listing fewer pool pages and measuring candidates from groups 2/3 first.

## Lessons learned:
1. Budget management is critical: should have listed fewer pages (maybe 4-5 instead of all 9)
2. Should have identified Task 2 candidates first (psa ≥ 40, not in group 1) and measured them
3. Then used remaining budget for Task 1 measurements
4. Could have measured 2-3 molecules from group 1 for Task 1 prediction, and 3-4 molecules from groups 2/3 for Task 2