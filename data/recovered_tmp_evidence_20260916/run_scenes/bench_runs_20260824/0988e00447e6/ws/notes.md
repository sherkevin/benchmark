# Notes

## Task Overview
- Task 1: Predict QED for m05 (not synthesized, must use trend from congeneric series)
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's series
- Budget: 12 units total (shared by list_pool and measure_qed)

## Approach Taken

### Step 1: Read the compound pool
Read all 9 pages of the pool to get all 33 molecules.
- Cost: 9 units
- Remaining budget: 3 units

### Step 2: Group by scaffold
Called group_by_scaffold (free) to identify congeneric series.
- m05 is in scaffold `COc1ccc(cc1)C(=O)N` with members: m15, m24, m28, m17, m22, m05
- Other scaffolds:
  - `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
  - `OCC(O)C(O)C(O)C(=O)N`: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)
  - `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: m04, m08, m11, m13, m16, m20, m23, m26, m30 (psa=29.1)

### Step 3: Measure m05's series for Task 1
Measured m15, m24, m28 to build a trend for predicting m05's QED.
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- Cost: 3 units
- Remaining budget: 0 units

m05 has chain_len=11. Used fit_trend with linear fit (degree=1):
- Prediction for m05: 0.7274

### Step 4: Task 2 - Budget Exhausted
**Problem**: Budget exhausted (12/12) before measuring any candidates for Task 2.

Candidates for Task 2 (psa ≥ 40, not in m05's series):
- Nitro series: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
- Sugar series: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)

No measurements were made on these molecules due to budget exhaustion.

## Strategic Error
I should have:
1. Called group_by_scaffold FIRST (before reading all pages)
2. Read fewer pages from list_pool (only needed to identify psa values)
3. Reserved budget for measuring Task 2 candidates

Better approach would have been:
- group_by_scaffold (free)
- Read ~4-5 pages of list_pool (4-5 units)
- Measure 2 molecules from m05's series (2 units)
- Measure 2-3 candidates for Task 2 (2-3 units)
- Total: 8-10 units, leaving 2-4 units buffer

## Results
- Task 1: Predicted QED for m05 = 0.7274 (linear fit on chain_len vs QED)
- Task 2: Could not complete - no budget left to measure candidates

## Final Answer
- target_qed: 0.7274
- lead_mol_id: null (no valid measurement)
- lead_qed_measured: null (no valid measurement)

Note: Task 2 could not be completed due to budget exhaustion. A proper strategy would have reserved budget for measuring candidates outside m05's series with psa ≥ 40.
