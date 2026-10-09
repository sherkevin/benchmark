# Task Notes

## Initial Analysis

### Data Overview
- Total molecules: 33
- 4 distinct scaffolds identified:
  1. COc1ccc(cc1)C(=O)N (methoxy phenyl series)
  2. O=[N+]([O-])c1ccc(cc1)C(=O)N (nitro phenyl series)
  3. OCC(O)C(O)C(O)C(=O)N (polyhydroxy series)
  4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (biphenyl series)

### Task 1: m05 QED Prediction
- m05 is in scaffold group 1 (COc1ccc(cc1)C(=O)N)
- Chain length: 11
- This molecule has never been synthesized, so we need to predict its QED from its congeneric series
- Need to measure other molecules in the same series to establish a trend

### Task 2: Finding a Lead Molecule
Constraints:
1. measured QED ≥ 0.6
2. psa ≥构成≥ 40
3. not in the same congeneric series as m05 (scaffold group 1)

We have 12 measurement budget total for both tasks.

## Strategy
1. First measure some molecules from m05's series (scaffold 1) to establish trend
2. Use fit_trend to predict m05 QED
3. Then search for lead molecule meeting all constraints

## Progress

### Measurements Taken (12/12 budget used)
Scaffold group 1 (m05's series):
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919  
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

Scaffold group 2 (nitro phenyl series):
- m02: QED = 0.6046 ✓ (psa=72.24 ≥ 40, not same series as m05)
- m12: QED = 0.4709
- m32: QED = 0.4683
- m09: QED = 0.5826
- m33: QED = 0.4591

Scaffold group 3 (polyhydroxy series):
- m14: QED = 0.3237
- m27: QED = 0.333

### m05 QED Predictions
- Linear fit (degree=1): 0.695
- Quadratic fit (degree=2): 0.5416
- Trend shows QED peaks at chain length 3 then decreases
- For chain length 11, 0.695 might be optimistic given decreasing trend

### Lead Molecule Candidate
- m02 meets all constraints: QED=0.6046 ≥ 0.6, psa=72.24 ≥ 40, not in same series as m05 (group 2 vs group 1)
- No other molecules measured with QED ≥ 0.6 from groups 2 or 3
- Group 4 eliminated (psa=29.1 < 40)
- Group 1 eliminated (same series as m05)

### Final Decisions
1. m05 QED prediction: Using linear fit (degree=1) on chain lengths 2-6 with QED values gives 0.695
2. Lead molecule: m02 (QED=0.6046, psa=72.24, scaffold group 2)

### Answer.json Values
- target_qed: 0.695
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046

All 12 measurement budget used.