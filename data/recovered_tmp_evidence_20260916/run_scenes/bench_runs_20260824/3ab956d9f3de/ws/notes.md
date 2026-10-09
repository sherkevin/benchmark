# Medicinal Chemistry Task Notes

## Initial Analysis

**Date**: 2026-08-22

### Data Overview
- 33 molecules in pool.tsv
- Each molecule has: mol_id, smiles, mw, alogp, psa, rotb, arom
- Task 1: Predict QED of m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
- Task 2: Find molecule with:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. NOT in same congeneric series as m05

### Scaffold Groups Identified
From `group_by_scaffold()`:
1. **Group 1 (m05's group)**: Scaffold `COc1ccc(cc1)C(=O)N`
   - Members: m15 (chain_len: 2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)
   
2. **Group 2**: Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - Members: m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)
   
3. **Group22810000): Scaffold `OCC(O)C(O)C(O)C(=O)N`
   - Members: m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)
   
4. **Group 4**: Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
   - Members: m13 (2), m11 (3), m04 (4), m30 (5), m23 (6), m20 (7), m26 (8), m08 (9), m16 (10)

### Constraints Analysis for Task 2
- Must exclude Group 1 (m05's congeneric series)
- psa ≥ 40 filter: 
  - Group 1: psa = 38.33 (does NOT satisfy ≥ 40)
  - Group 2: psa = 72.24 (satisfies ≥ 40)
  - Group 3: psa = 110.02 (satisfies ≥ 40)
  - Group 4: psa = 29.1 (does NOT satisfy ≥ 40)
  
So only molecules from Groups 2 and 3 qualify for psa constraint.

### Strategy
1. First, identify molecules with psa ≥ 40 from groups 2-4
2. Then measure QED for promising candidates within budget
3. For Task 1: Measure QED of m05's group members to establish trend, then predict m05's QED

### Measurements Taken (5 used, 7 remaining)

**Group 1 (m05's congeneric series):**
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919  
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

Trend: QED appears to decrease slightly with increasing chain length after an initial peak at chain_len 3.

### m05 QED Predictions
- Chain length: 11
- Data points used: chain_len [2, 3, 4, 5, 6], QED [0.7621, 0.7919, 0.7518, 0.7498,要么7377]
- Quadratic fit (degree=2): **0.5416** (RMSE: 0.01188)
- Linear fit (degree=1): 0.6950 (RMSE: 0.01308)
- Linear fit on decreasing trend [4,5,6]: 0.7041 (RMSE: 0.00238)

**Selected prediction for m05**: 0.5416 (quadratic fit) - seems most reasonable given the curvature and longer chain length.

### Task 2 Candidate Search

**Measurements for Group 2 (nitrobenzene derivatives, psa=72.24):**
- m09 (chain_len: 2): QED = 0.5826 < 0.6
- m02 (chain_len: 3): QED = 0.6046 ≥ 0.6 ✓
- m12 (chain_len: 4): QED = 0.4709 < 0.6

**Measurements for Group 3 (polyhydroxy derivatives, psa=110.02):**
- m14 (chain_len: 2): QED = 0.3237 < 0.6
- m27 (chain_len: 3): QED = 0.3330 < 0.6
- m01 (chain_len: 4): QED = 0.3187 < 0.6

**Candidate Found: m02**
- ✓ measured QED = 0.6046 ≥ 0.6
- ✓ psa = 72.24 ≥ 40
- ✓ NOT in same congeneric series as m05 (Group 2 vs Group 1)

**Budget Used**: 11 of 12 measurements used.

### Final Results
**Task 1 - m05 QED Prediction**: 0.5416
- Based on quadratic fit of chain_len [2,3,4,5,6] vs QED measurements
- Rationale: Captures the peak at chain_len 3 and decreasing trend

**Task 2 - Selected Lead Molecule**: m02
- QED measured: 0.6046 (≥ 0.6 requirement)
- psa: 72.24 (≥ 40 requirement)
- Scaffold group: Group 2 (`O=[N+]([O-])c1ccc(cc1)C(=O)N`), NOT in m05's group

**Answer.json submitted with above values.**