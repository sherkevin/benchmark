# Notes

## Task Overview
- Task 1: Predict QED of m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) using congeneric series trend
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's series

## Budget: 12 units total (shared between list_pool and measure_qed)

## Actions Taken

### 1. Read the compound pool (9 pages, 9 budget units)
Read all 33 molecules across 9 pages. This was inefficient - I should have been more selective.

### 2. Group by scaffold (free)
Identified 4 scaffold groups:
- COc1ccc(cc1)C(=O)N (m05's series): m15, m24, m28, m17, m22, m05 - psa=38.33
- O=[N+]([O-])c1ccc(cc1)C(=O)N (nitro series): m02, m06, m09, m10, m12, m29, m31, m32, m33 - psa=72.24
- OCC(O)C(O)C(O)C(=O)N (sugar series): m01, m03, m07, m14, m18, m19, m21, m25, m27 - psa=110.02
- c1ccc(cc1)-c1ccc(cc1)C(=O)N (biphenyl series): m04, m08, m11, m13, m16, m20, m23, m26, m30 - psa=29.1

### 3. Measured molecules in m05's series (3 budget units)
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518

Budget exhausted after m28. Attempts to measure m17 and m22 failed.

### 4. Trend fitting for m05 (free)
Used linear fit (degree=1) with points:
- (2, 0.7621), (3, 0.7919), (4, 0.7518)

Prediction for m05 (chain_len=11): QED = 0.7274

## Strategic Error
I made a critical mistake by:
1. Reading all 9 pages first (9 units)
2. Using all remaining 3 units on m05's series molecules
3. Leaving 0 units for measuring potential lead candidates

Optimal strategy would have been:
1. Read fewer pages or be selective (e.g., only pages with molecules to measure)
2. Measure 2 molecules in m05's series for trend (2 units)
3. Measure 1 molecule from nitro or sugar series for lead (1 unit)
Total: ~6-9 units for pages + 3 for measurements = within budget

## Task 1 Result
Predicted QED for m05: **0.7274** (from linear trend fit)

## Task 2 Result
**Cannot complete** - I have no measured QED values for molecules outside m05's series. All my measurements (m15, m24, m28) are in m05's series and have psa=38.33 < 40, so they don't qualify.

Eligible series for Task 2 (psa ≥ 40, not m05's series):
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

But I have no measurements for any of these molecules due to budget exhaustion.

## Lessons Learned
1. Use free tools (group_by_scaffold) first to understand the data structure
2. Be selective about which pages to read - don't read all pages upfront
3. Reserve budget for both tasks - don't spend all on one task
4. For Task 2, need to measure molecules from nitro or sugar series (psa ≥ 40)

## Final Answer
- target_qed: 0.7274 (predicted for m05 using linear trend fit)
- lead_mol_id: null (could not complete Task 2 due to budget exhaustion)
- lead_qed_measured: null (no measurements of eligible molecules)

The answer.json file has been written with these values. Task 2 could not be completed because all budget was spent on reading all 9 pages and measuring molecules in m05's series, leaving no budget for measuring potential lead candidates from eligible series (nitro or sugar scaffolds with psa ≥ 40).
