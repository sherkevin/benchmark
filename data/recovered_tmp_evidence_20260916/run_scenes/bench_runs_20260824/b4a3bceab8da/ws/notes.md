# Medicinal Chemistry Task Notes

## Initial Analysis
- Total molecules: 33 in data/pool.tsv
- m05 target molecule: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold analysis shows 4 distinct scaffolds
- m05 belongs to scaffold "COc1ccc(cc1)C(=O)N" with chain length 11
- Same series members: m15 (chain_len:2), m24 (chain_len:3), m28 (chain_len:4), m17 (chain_len:5), m22 (chain_len:6)
- Total measurement budget: 12 measurements for both tasks

## Task 1 Strategy
To predict QED of m05 (chain length 11):
1. Measure QED of other molecules in same series to establish trend
2. Use fit_trend to predict m05 QED based on chain length vs QED relationship
3. Need to decide how many measurements to use vs saving for Task 2

## Measurements Made (Task 1)
1. m15 (chain_len:2): QED = 0.7621
2. m24 (chain_len:3): QED = 0.7919
3. m28 (chain_len:4): QED = 0.7518
4. m17 (chain_len:5): QED = 0.7498
5. m22 (chain_len:6): QED = 0.7377

Observations: QED seems to peak around chain length 3, then decreases with longer chains.

## m05 Predictions
Chain length: 11
- Degree 1 (linear) fit: 0.695
- Degree 2 (quadratic) fit: 0.5416  
- Degree 3 (cubic) fit: 2.1323 (invalid, >1)

Quadratic seems most reasonable given decreasing trend. Cubic overfits and gives invalid value >1.

## Task 2 Search
Scaffolds considered:
- Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): psa = 72.24
- Scaffold 3 (OCC(O)C(O)C(O)C(=O)N): psa = 110.02
.### Scaffold 2 Measurements:
- m09 (chain_len:2): QED = 0.5826
- m02 (chain_len:3): QED = 0.6046 ✓ (≥0.6)
- m12 (chain_len:4): QED = 0.4709
- m32 (chain_len:5): QED = 0.4683

### Scaffold 3 Measurements:
- m14 (chain_len:2): QED = 0.3237
- m27 (chain_len:3): QED = 0.333
- m01 (chain_len:4): QED = 0.3187

Best candidate found: m02 with QED =会影响 0.6046 ≥ 0.6, psa = 72.24 ≥ 40, and not in m05's series.

## Final Decisions
### Task 1: m05 QED Prediction
Selected quadratic fit (degree 2) using all 5 data points from same series:
- Chain lengths: [2, 3, 4, 5, 6]
- QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Prediction for chain length 11: 0.5416

Rationale: Quadratic fit captures the decreasing trend and potential acceleration of QED decline with longer alkyl chains. Linear fit (0.695) seems optimistic given that QED already shows downward trend.

### Task 2: Lead Molecule Selection
Selected: m02
- QED: 0.6046 (measured) ≥ 0.6 ✓
- psa: 72.24 (from data) ≥ 40 ✓
- Series: Scaffold 2, different from m05's scaffold 1 ✓

Alternative scaffolds (scaffold 3) showed consistently low QED (< 0.6).

## Budget Usage
Total measurements used: 12/12
- Task 1 series: 5 measurements (m15, m24, m28, m17, m22)
- Task 2 search: 7 measurements (m09, m02, m14, m27, m01, m12, m32)

## Task 2 Requirements
Find molecule with:
1. measured QED ≥ 0.6
2. psa ≥ 40  
3. NOT in same congeneric series as m05 (scaffold "COc1ccc(cc1)C(=O)N")

## Initial Observations
Looking at psa values in data:
- Scaffold 1 (COc1ccc(cc1)C(=O)N): all have psa = 38.33
- Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): all have psa = 72.24
- Scaffold 3 (OCC(O)C(O)C(O)C(=O)N): all have psa = 110.02
- Scaffold 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N): all have psa = 29.1

For Task 2, need psa ≥ 40, so scaffolds 1 and 4 are eliminated (psa too low).
Scaffolds 2 (psa=72.24) and 3 (psa=110.02) are candidates.