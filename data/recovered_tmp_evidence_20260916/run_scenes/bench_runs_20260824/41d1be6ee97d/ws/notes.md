# Analysis and Approach

## Identifying Series

From SMILES analysis, four distinct congeneric series are present:

1. **Polyhydroxy series**: `OCC(O)C(O)C(O)C(=O)N` scaffold (m01, m03, m07, m14, m18, m19, m21, m25, m27)
2. **Nitrobenzene series**: `O=[N+]([O-])c1ccc(cc1)C(=O)N` scaffold (m02, m06, m09, m10, m12, m29, m31, m32, m33)
3. **Biphenyl series**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` scaffold (m04, m08, m11, m13, m16, m20, m23, m26, m30)
4. **Methoxybenzene series**: `COc1ccc(cc1)C(=O)N` scaffold (m05, m15, m17, m22, m24, m28)

**Target molecule m05** belongs to the **methoxybenzene series**.

## Plan for Job 1: Predicting QED of m05

### Step 1: Identify series members
From the methoxybenzene series, the pool contains:
- m15: `COc1ccc(cc1)C(=O)NCC` (chain length: C2)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain length: C3)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain length: C4)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain length: C5)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length: C6)

m05 has chain length C13: `NCCCCCCCCCCC` (13 carbons)

### Step 2: Measurement strategy
We have 12 total measurements available. I need to measure QED values for methoxybenzene series members to establish a trend. Since the problem states we're extrapolating beyond the measured range (pool only has short-chain members), I should measure all available series members to get a good trend.

Available measurements: m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6) = 5 measurements

### Step 3: Trend fitting
Will measure QED for these 5 molecules, then use `fit_trend` with degree=2 (quadratic) to extrapolate to chain length 13 (m05).

## Measurements Completed (Job 1)
Measured QED for methoxybenzene series members:
1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377

Chain lengths (xs): [2, 3, 4, 5, 6]
QED values (ys): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Target m05 has chain length 13 (C13).

## Trend Fitting Results
Using quadratic fit (degree=2) with xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377]:
- Prediction for x_query=13: **QED = 0.419**
- RMSE: 0.01188 (better fit than linear RMSE=0.01308)
- Linear prediction: 0.6768

Quadratic fit is better (lower RMSE) and doesn't assume linearity, so using 0.419 as predicted QED for m05.

## Plan for Job 2: Selecting Backup Lead

### Constraints:
1. Measured QED ≥ 0.6 (requires measurement)
2. PSA ≥ 40 (free from data)
3. Not in same series as m05 (methoxybenzene series)

### Approach:
1. First use `screen_candidates()` to get molecules with PSA ≥ 40
2. Filter out methoxybenzene series members
3. From remaining candidates, measure QED to find one with QED ≥ 0.6
4. Have 7 measurements remaining after Job 1 (12 total - 5 for Job 1)

### Budget consideration: 12 total measurements
- Job 1: 5 measurements for methoxybenzene series
- Remaining: 7 measurements for lead screening
- Need to be efficient in lead selection

## Job 2: Lead Selection Results

### Screening Results
- 18 candidates pass PSA ≥ 40 filter from `screen_candidates()`
- All are NOT in methoxybenzene series (constraint 3 automatically satisfied)
- Candidates belong to either polyhydroxy or nitrobenzene series

### Measurements for Lead
1. Measured m01 (polyhydroxy series): QED = 0.3187 (< 0.6, fails)
2. Measured m02 (nitrobenzene series): QED = 0.6046 (≥ 0.6, passes!)

### Selected Lead: m02
- **mol_id**: m02
- **QED**: 0.6046 (≥ 0.6 constraint satisfied)
- **PSA**: 72.24 (≥ 40 constraint satisfied)
- **Series**: Nitrobenzene series (not methoxybenzene, constraint 3 satisfied)

m02 satisfies all three constraints and is a valid backup lead compound.

## Summary

### Job 1 Complete: Predicted QED for m05
- Identified methoxybenzene series congeners: m15, m24, m28, m17, m22
- Measured their QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377] for chain lengths [2,3,4,5,6]
- Used quadratic fit (degree=2) to extrapolate to chain length 13 (m05)
- **Predicted QED for m05: 0.419**

### Job 2 Complete: Selected Backup Lead
- Screened candidates with PSA ≥ 40: 18 molecules
- All automatically satisfy "not in same series as m05" constraint
- Measured m01 (QED=0.3187, too low)
- Measured m02 (QED=0.6046, meets ≥0.6 threshold)
- **Selected lead: m02** with measured QED=0.6046

### Budget Usage
- Total budget: 12 measurements
- Used: 7 measurements (5 for Job 1, 2 for Job 2)
- Remaining: 5 measurements unused

### Final Answer
- target_qed: 0.419
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]