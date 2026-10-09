# Notes for Compound Analysis

## Task Overview
1. Predict QED of m05 (target molecule, not synthesized)
2. Find a molecule with:
   - QED ≥ 0.6
   - PSA ≥ 40
   - Not in same congeneric series as m05

## Initial Findings
- Total pool: 33 molecules
- Budget: 12 units total (used 2 for reading pages 1-2, 10 remaining)
- m05 found on page 2: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, chain length 11

## Congeneric Series Analysis
From `group_by_scaffold()`:
- m05 belongs to scaffold "COc1ccc(cc1)C(=O)N"
- Other members: m15 (chain_len 2), m24 (3), m28 (4), m17 (5), m22 (6)
- Total 4 scaffold groups in the pool

## QED Measurements for m05's Series
Measured QED for m05 congeneric series members:
1. m15 (chain_len 2): QED = 0.7621
2. m24 (chain_len 3): QED = 0.7919  
3. m28 (chain_len 4): QED = 0.7518
4. m17 (chain_len 5): QED = 0.7498
5. m22 (chain_len 6): QED = 0.7377

## m05 QED Predictions
Using chain length as x and QED as y:
- Linear fit (degree 1): predicted QED = 0.6950
- Quadratic fit (degree 2): predicted QED = 0.5416
- Linear fit seems more reasonable given the trend

## Task 2 Candidate Search
Scaffolds with PSA ≥ 40:
1. Nitrobenzene scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N): PSA = 72.24
   - Candidates: m02, m06, m09, m10, m12
2. Polyol scaffold (OCC(O)C(O)C(O)C(=O)N): PSA = 110.02
   - Candidates: m01, m03, m07, m14, m18, m19

Biphenyl scaffold has PSA = 29.1 (< 40) for all seen members.

Budget status: 11 used (5 pages + 6 measurements), 1 remaining

## Lead Molecule Found: m02
- QED measured: 0.6046 (≥ 0.6) ✓
- PSA: 72.24 (≥ 40) ✓  
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold) ✓
- All three constraints satisfied!

## Final Results
1. m05 QED prediction: 0.6950 (based on linear fit of congeneric series QED vs chain length)
2. Lead molecule: m02 with measured QED 0.6046