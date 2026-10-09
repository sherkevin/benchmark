# Task Notes

## Initial Analysis
Looking at the data in `data/pool.tsv`, I can see 33 molecules with various properties. Key observations:

1. **Task 1**: Need to predict QED for `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). This molecule has never been synthesized, so `measure_qed` will return `probe_unavailable`.

2. **Task 2**: Need to find a molecule that satisfies:
   - measured QED ≥ 0.6
   - `psa` ≥ 40
   - not in the same congeneric series as `m05` (same scaffold, differ only in N-alkyl chain length)

3. **Budget**: 12 total measurements shared between both tasks.

## Understanding Congeneric Series
From the SMILES patterns, I can identify several series:

1. **m05 series** (methoxybenzamide with varying N-alkyl chain): m15, m17, m22, m24, m28
   - Scaffold: `COc1ccc(cc1)C(=O)N` followed by alkyl chain
   - Examples: m15 (NCC), m17 (NCCCCC), m22 (NCCCCCC), m24 (NCCC), m28 (NCCCC)

2. **Other series**:
   - O=[N+]([O-])c1ccc(cc1)C(=O)N series (nitrobenzamides): m02, m06, m09, m10, m12, m29, m31, m32, m33
   - c1ccc(cc1)-c1ccc(cc1)C(=O)N series (biphenyl amides): m04, m08, m11, m13, m16, m20, m23, m26, m30
   - OCC(O)C(O)C(O)C(=O)N series (tetrahydroxybutanamides): m01, m03, m07, m14, m18, m19, m21, m25, m27

m05 has psa = 38.33 (< 40), so it wouldn't satisfy Task 2 anyway.

## Strategy
1. First identify candidate molecules for Task 2 with psa ≥ 40
2. Measure QED for some representatives from different series
3. Use measurements to establish trends for QED prediction for Task 1
4. Find a molecule satisfying all Task 2 criteria within budget

## Measurements Taken
1. m05: probe_unavailable (0 budget used)
2. m15: QED = 0.7621 (1 budget used)
3. m17: QED = 0.7498 (2 budget used)
4. m22: QED = 0.7377 (3 budget used)
5. m24: QED = 0.7919 (4 budget used)
6. m28: QED = 0.7518 (5 budget used)
7. m01: QED = 0.3187 (6 budget used)
8. m02: QED = 0.6046 (7 budget used)
9. m09: QED = 0.5826 (8 budget used)
10. m14: QED = 0.3237 (9 budget used)
11. m12: QED = 0.4709 (10 budget used)
12. m32: QED = 0.4683 (11 budget used)
13. m29: QED = 0.4446 (12 budget used)

Total budget used: 12, remaining: 0

## m05 Series Analysis
Molecule | SMILES fragment (alkyl chain) | Chain length (carbons) | QED | mw | alogp
--- | --- | --- | --- | --- | ---
m15 | NCC | 2 | 0.7621 | 179.22 | 1.445
m24 | NCCC | 3 | 0.7919 | 193.25 | 1.835
m28 | NCCCC | 4 | 0.7518 | 207.27 | 2.225
m17 | NCCCCC | 5 | 0.7498 | 221.30 | 2.615
m22 | NCCCCCC | 6 | 0.7377 | 235.33 | 3.005
m05 | NCCCCCCCCCCC | 12 | ? | 305.46 | 4.956

## Task 2 Candidate Analysis
Found candidate: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05 series (nitrobenzamide vs methoxybenzamide) ✓

Other nitrobenzamides measured:
- m09 (2C): 0.5826 < 0.6
- m12 (4C): 0.4709 < 0.6
- m32 (5C): 0.4683 < 0.6
- m29 (7C): 0.4446 < 0.6

Tetrahydroxybutanamides have very low QED (~0.32).

Thus m02 is the best candidate found.

## m05 QED Prediction
Based on trend analysis of m05 series:
- QED peaks at 3-carbon chain (0.7919)
- Decreases with longer chains due to increasing alogp and molecular weight
- Average QED decrease per carbon beyond 3C: ~0.018
- From 6C (m22, QED=0.7377) to 12C (m05): 6 more carbons
- Predicted decrease: 6 × 0.018 = 0.108
- Predicted QED: 0.7377 - 0.108 = 0.6297 ≈ 0.63

Considering alogp increase (4.956 vs 3.005 for m22) and the nonlinear relationship, final prediction: **0.645**

## Final Answer
- target_qed (m05): 0.645
- lead_mol_id: m02
- lead_qed_measured: 0.6046

All constraints satisfied for Task 2:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05 series (different scaffold) ✓