# Task Notes

## Initial Analysis
- Need to predict QED for m05 (target molecule) - never synthesized, can't be measured directly
- Need to find one molecule that satisfies all 3 constraints:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. not in same congeneric series as m05 (different scaffold or same scaffold with different N-alkyl chain length variation)

## Scaffold Groups Found
1. Scaffold 1: "COc1ccc(cc1)C(=O)N" - contains m05 (chain_len 11) and m15(2), m24(3), m28(4), m17(5), m22(6)
2. Scaffold 2: "O=[N+]([O-])c1ccc(cc1)C(=O)N" - nitrobenzene series
3. Scaffold 3: "OCC(O)C(O)C(O)C(=O)N" - sugar-like series
4. Scaffold 4: "c1ccc(cc1)-c1ccc(cc1)C(=O)N" - biphenyl series

## Approach
1. First, use group_by_scaffold() to identify different scaffolds and find which molecules share scaffold with m05 ✓
2. For m05 prediction: need to measure QED of molecules in same congeneric series (same scaffold, different N-alkyl chain length) to establish trend ✓
3. For task 2: need to identify molecules with psa ≥ 40, measure their QED, check they're not in same series as m05
4. Total budget: 12 measurements shared between both tasks

## Task 1 Progress
Measured 5 molecules in same series as m05 (Scaffold 1):
- m15 (chain_len 2): QED = 0.7621
- m24 (chain_len 3): QED = 0.7919  
- m28 (chain_len 4): QED = 0.7518
- m17 (chain_len 5): QED = 0.7498
- m22 (chain_len 6): QED = 0.7377

m05 has chain_len 11. QED seems to decrease with longer chains due to increased MW and rotatable bonds.

Predictions:
- Linear fit (degree=1): QED = будет 0.695
- Quadratic fit (degree=2): QED = 0.5416

Decision: Using quadratic fit prediction of 0.5416 because:
1. Trend shows decrease with chain length
2. QED components (especially ROTB) degrade significantly with longer chains
3. m05 has chain_len 11 (far beyond measured range 2-6)
4. Quadratic RMSE (0.01188) slightly better than linear (0.01308)

## Task 2 Progress
Measured molecules to find candidate satisfying all constraints:
1. psa ≥ 40: Scaffold 2 (psa=72.24) and Scaffold 3 (psa=110.02) qualify
2. Not in same series as m05: Scaffold 2 and 3 both different from Scaffold 1
3. measured QED ≥ 0.6

Found candidate: m02
- QED measured: 0.6046 (≥ 0.6)
- psa: 72.24 (≥ 40)
- Scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (different from m05's scaffold)
- Satisfies all 3 constraints

Alternative candidates measured:
- m09: QED=0.5826 (< 0.6)
- m12: QED=0.4709 (< 0.6)
- m32: QED=0.4683 (< 0.6)
- m33: QED=0.4591 (< 0.6)
- Scaffold 3 molecules (m14, m27): QED too low (~0.33)

## Budget Usage
Total measurements: 12/12 used
- Task 1: 5 measurements (m15, m24, m28, m17, m22)
- Task 2: 7 measurements (m14, m27, m09, m02, m12, m32, m33)

## Final Answer
- target_qed (m05): 0.5416 (predicted using quadratic fit on chain_len 2-6 QED data)
- lead_mol_id: m02 (satisfies all 3 constraints)
- lead_qed_measured: 0.6046 (actual measurement)

Task completed successfully!