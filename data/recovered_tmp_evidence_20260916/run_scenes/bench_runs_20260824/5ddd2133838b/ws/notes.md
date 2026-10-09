# Notes for Medicinal Chemistry Task

## Initial Setup
- Date: 2026-08-22
- Task: Predict QED for target molecule m05 and select a lead molecule meeting constraints
- Total budget: 12 units (shared between list_pool and measure_qed)
- Target molecule m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Constraints for lead molecule:
  1. measured QED ≥ 0.6
  2. `psa` ≥ 40  
  3. not in same congeneric series as m05 (same scaffold, differing only in N-alkyl chain length)

## Plan
1. Explore compound pool using list_pool to understand the molecules available
2. Identify which molecules are in same congeneric series as m05 (same scaffold)
3. Measure QEDs for promising candidates to find lead meeting all constraints
4. Predict QED for m05 using trend from its congeneric series
5. Fill answer.json with results

## Budget Tracking
Initial budget: 12 units
Used: 9 units (all 9 pages of list_pool)
Remaining: 3 units

## Findings from Compound Pool
Scanned all 33 molecules across 9 pages (4 per page, except last page has 1).

### Congeneric Series of m05 (methoxy-phenyl amides):
Found these molecules with scaffold `COc1ccc(cc1)C(=O)N...`:
1. m15: `COc1ccc(cc1)C(=O)NCC` (ethyl chain, 2C)
2. m24: `COc1ccc(cc1)C(=O)NCCC` (propyl chain, 3C)
3. m28: `COc1ccc(cc1)C(=O)NCCCC` (butyl chain, 4C)
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` (pentyl chain, 5C)
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (hexyl chain, 6C)
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (dodecyl chain, 12C)

### Other Series Identified:
1. Nitro-phenyl amides: `O=[N+]([O-])c1ccc(cc1)C(=O)N...` (m02, m06, m09, m10, m12, m29, m31, m32, m33) - psa = 72.24
2. Biphenyl amides: `c1ccc(cc1)-c1ccc(cc1)C(=O)N...` (m04, m08, m11, m13, m16, m20, m23, m26, m30) - psa = 29.1
3. Polyhydroxy amides: `OCC(O)C(O)C(O)C(=O)N...` (m01, m03, m07, m14, m18, m19, m21, m25, m27) - psa = 110.02

## QED Measurements
### m05 Series Measurements:
1. m15 (ethyl, 2C): QED = 0.7621
2. m22 (hexyl, 6C): QED = 0.7377

### Trend Analysis:
Alkyl chain lengths vs QED:
- 2C: 0.7621
- 6C: 0.7377

QED appears to decrease slightly with increasing alkyl chain length. Need to predict for 12C chain (m05).

### Lead Candidate Found:
m02 meets all constraints:
1. QED = 0.6046 (≥ 0.6) ✓
2. psa = 72.24 (≥ 40) ✓  
3. Not in m05 series (nitro-phenyl vs methoxy-phenyl) ✓

### Budget Summary:
- list_pool: 9 units (pages 1-9)
- measure_qed: 3 units (m15, m22, m02)
- Total: 12/12 units used

## Final Results
### Task 1: Predict QED for m05
Based on linear extrapolation from m15 (2C chain, QED=0.7621) and m22 (6C chain, QED=0.7377):
- QED decrease per carbon: (0.7621 - 0.7377) / 4 = 0.0061
- m05 has 12C chain vs m22's 6C: +6 carbons
- Predicted additional decrease: 6 × 0.0061 = 0.0366
- Predicted QED for m05: 0.7377 - 0.0366 = 0.7011 ≈ 0.701

### Task 2: Select Lead Molecule
Selected m02 which satisfies all constraints:
1. Measured QED = 0.6046 (≥ 0.6) ✓
2. psa = 72.24 (≥ 40) ✓
3. Not in same congeneric series as m05 (nitro-phenyl scaffold vs methoxy-phenyl) ✓

## Answer
Written to answer.json:
- target_qed: 0.701 (predicted QED for m05)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046