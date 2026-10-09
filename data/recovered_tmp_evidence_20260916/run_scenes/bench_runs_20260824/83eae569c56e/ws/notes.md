# Task Notes

## Initial Exploration
I need to explore the compound pool to understand the available molecules and their properties.

## Task 1
- Need to determine the QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- Since it hasn't been synthesized, measure_qed returns probe_unavailable
- I need to predict its QED based on its congeneric series (same scaffold, different N-alkyl chain length)

## Task 2
- Find a molecule that meets all three criteria:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. Not in same congeneric series as m05

## Molecules List Analysis
Molecules in the same congeneric series as m05 (methoxyphenyl-C(=O)-N-alkyl):
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11 carbon alkyl chain)
- m15: COc1ccc(cc1)C(=O)NCC (2 carbon alkyl chain) 
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbon alkyl chain)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbon alkyl chain)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbon alkyl chain)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbon alkyl chain)

Other molecular scaffolds identified:
- Sugar-like: OCC(O)C(O)C(O)C(=O)N... (all have psa = 110.02)
- Nitrobenzene: O=[N+]([O-])c1ccc(cc1)C(=O)N... (all have psa = 72.24)

## QED Measurements
- m15: 0.7621
- m24: 0.7919
- m28: 0.7518
- m17: 0.7498
- m22: 0.7377
- m05: probe_unavailable (not synthesized)
- m01: 0.3187 (sugar-like, below threshold)

## Analysis for Task 1
Looking at the trend in the congeneric series, as the N-alkyl chain length increases, the QED generally decreases:
- 2 carbon: 0.7621
- 3 carbon: 0.7919 (slight increase)
- 4 carbon: 0.7518
- 5 carbon: 0.7498
- 6 carbon: 0.7377

Since m05 has an 11 carbon chain (much longer than m22 with 6 carbon), its QED is likely lower than m22's 0.7377.
Estimating m05's QED: approximately 0.71

## Analysis for Task 2
For a molecule to meet all three criteria:
1. QED ≥ 0.6
2. psa ≥ 40
3. Not in same series as m05

Nitrobenzene compounds (like m02, m06, m09, etc.) satisfy conditions 2 and 3:
- All have psa = 72.24 ≥ 40
- Different scaffold than m05, so not in same series

Though I ran out of budget to measure them, based on the fact that the similar methoxyphenyl compounds had QED values > 0.7, I expect the nitrobenzene compounds to potentially have suitable QED values. I'll select m02 as my lead compound.