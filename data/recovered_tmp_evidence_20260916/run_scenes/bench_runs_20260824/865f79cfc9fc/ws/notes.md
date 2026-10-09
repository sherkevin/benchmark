# Notes for Medicinal Chemistry Task

## Task Overview
1. Report QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - this molecule has never been synthesized
2. Pick one molecule from pool that satisfies:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in same congeneric series as m05 (same scaffold, differ only in N-alkyl chain length)

Budget: 12 measurements total for both tasks.

## Initial Analysis

From data/pool.tsv:
- m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, psa=38.33, rotb=12, arom=1
- Need to identify congeneric series with m05: molecules with scaffold `COc1ccc(cc1)C(=O)N...` (methoxy-phenyl-carbonyl-N-alkyl chain)
- Looking at pool, I see several molecules with this pattern:
  - m15: COc1ccc(cc1)C(=O)NCC
  - m17: COc1ccc(cc1)C(=O)NCCCCC
  - m22: COc1ccc(cc1)C(=O)NCCCCCC
  - m24: COc1ccc(cc1)C(=O)NCCC
  - m28: COc1ccc(cc1)C(=O)NCCCC
  - m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (longest chain)

These all appear to be in the same congeneric series (same scaffold, varying N-alkyl chain length).

## Strategy
1. First understand the relationship between descriptors and QED in the pool
2. Measure QED for molecules that are NOT in m05's series and have psa ≥ 40
3. Find patterns to predict m05's QED
4. Use budget efficiently (12 measurements total)

## Molecules with psa ≥ 40
From quick scan:
- psa ≥ 40: molecules with high polar surface area
- Looking at the pool, I see several groups:
  1. OCC(O)C(O)C(O)C(=O)N... series (polyol-carbamates): psa=110.02
  2. O=[N+]([O-])c1ccc(cc1)C(=O)N... series (nitrophenyl-carbamates): psa=72.24
  3. COc1ccc(cc1)C(=O)N... series (methoxy-phenyl-carbamates): psa=38.33
  4. c1ccc(cc1)-c1ccc(cc1)C(=O)N... series (biphenyl-carbamates): psa=29.1

So psa ≥ 40 eliminates series 3 and 4. Only series 1 and 2 have psa ≥ 40.

## Next Steps
1. Check which molecules are NOT in m05's series and have psa ≥ 40
2. Start measuring QED for representative molecules to understand trends
3. Use trend to predict m05 QED

## Measurements Taken (12/12 used)

### Series B (polyol-carbamates, psa=110.02)
1. m01: QED = 0.3187 (chain: NCCCC)
2. m03: QED = 0.3282 (chain: NCCCCCC)
3. m07: QED = 0.3233 (chain: NCCCCCCCCC)
4. m25: QED = 0.3148 (chain: NCCCCCCCCCC)

Observations: Series B has consistently low QED (~0.31-0.33), all below 0.6 threshold.

### Series C (nitrophenyl-carbamates, psa=72.24)
5. m02: QED = 0.6046 (chain: NCCC) ✓ Meets QED ≥ 0.6 requirement!
6. m09: QED = 0.5826 (chain: NCC) - slightly below 0.6
7. m12: QED = 0.4709 (chain: NCCCC)
8. m32: QED = 0.4683 (chain: NCCCCC)
9. m33: QED = 0.4591 (chain: NCCCCCC)
10. m29: QED = 0.4446 (chain: NCCCCCCC)

Observations: Series C shows decreasing QED with increasing alkyl chain length. m02 has highest QED (0.6046) and meets requirements.

### Series A (methoxy-phenyl-carbamates, psa=38.33) - m05's series
11. m15: QED = 0.7621 (chain: NCC)
12. m17: QED = 0.7498 (chain: NCCCCC)

Observations: Series A has higher QED values (~0.75-0.76). Shows slight decrease with longer chain.

## Analysis for Task 1: Predicting m05 QED
m05 has the longest alkyl chain in Series A (NCCCCCCCCCCC, 11 carbons in the alkyl chain excluding the carbonyl-N).
From measured data:
- m15 (NCC): 0.7621 (2 carbons in alkyl chain)
- m17 (NCCCCC): 0.7498 (5 carbons in alkyl chain)

Pattern: QED decreases slightly with increasing chain length. m05 has much longer chain (11 carbons), so QED should be lower.

Chain length vs QED in Series A:
- 2 carbons: 0.7621
- 5 carbons: 0.7498
- 11 carbons: ? (predicted)

The decrease from 2 to 5 carbons is 0.0123 over 3 additional carbons (-0.0041 per carbon). Extrapolating to 11 carbons: decrease of 9 additional carbons × -0.0041 ≈ -0.0369 from m15's 0.7621.
Predicted m05 QED: 0.7621 - 0.0369 ≈ 0.7252

## Analysis for Task 2: Selecting lead molecule
Requirements:
1. measured QED ≥ 0.6 ✓
2. psa ≥ 40 ✓  
3. not in same series as m05 ✓

Only m02 meets all requirements:
- QED = 0.6046 (≥ 0.6)
- psa = 72.24 (≥ 40)
- Series C (nitrophenyl-carbamates), not Series A (methoxy-phenyl-carbamates)

All other measured molecules either have QED < 0.6 (Series B, most of Series C) or are in m05's series (Series A).

## Final Decisions

### Task 1: m05 predicted QED
Based on Series A trend:
- m15 (2-carbon chain): QED = 0.7621
- m17 (5-carbon chain): QED = 0.7498
- Difference: -0.0123 for +3 carbons = -0.0041 per carbon

m05 has 11-carbon chain (9 more carbons than m17):
Predicted decrease: 9 × (-0.0041) ≈ -0.0369
Predicted QED: 0.7498 - 0.0369 = 0.7129

Considering additional factors:
.Molecular weight increases significantly (179→221→305)
.Rotatable bonds increase substantially (3→6→12)
.Both trends typically decrease QED

Conservative estimate: 0.715 (rounded from 0.7129)

### Task 2: Lead molecule selection
Candidate: m02
Reasons:
1. **QED ≥ 0.6**: Measured QED = 0.6046 ✓
2. **psa ≥ 40**: psa = 72.24 ✓  
3. **Not in same series as m05**: Series C (nitrophenyl-carbamates) vs Series A (methoxy-phenyl-carbamates) ✓

Alternative candidates considered:
- Series B (polyol-carbamates): All QED < 0.6 (0.31-0.33)
- Other Series C molecules: Most have QED < 0.6, except m02
- Series A (m05's series): Excluded by requirement #3
- Series D (biphenyl-carbamates): psa = 29.1 (< 40)

m02 is the only measured molecule meeting all three criteria.

## Final Answer
Written to answer.json:
- target_qed: 0.715 (predicted m05 QED)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046