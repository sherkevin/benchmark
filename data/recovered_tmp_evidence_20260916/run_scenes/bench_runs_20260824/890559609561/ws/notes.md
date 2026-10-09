# Notes

## Task Overview
- Task 1: Predict QED of m05 (not synthesizable) from its congeneric series
- Task 2: Find one molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's scaffold series
- Budget: 12 units total (list_pool and measure_qed share budget)

## Pool Exploration
Read all 9 pages of the pool (33 molecules total). Budget used: 9 units.

## Scaffold Groups (from group_by_scaffold)
1. **m05's series** (COc1ccc(cc1)C(=O)N): m15, m24, m28, m17, m22, m05 - psa=38.33
2. **Nitro series** (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09, m02, m12, m32, m33, m29, m31, m06, m10 - psa=72.24
3. **Sugar series** (OCC(O)C(O)C(O)C(=O)N): m14, m27, m01, m19, m03, m21, m18, m07, m25 - psa=110.02
4. **Biphenyl series** (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13, m11, m04, m30, m23, m20, m26, m08, m16 - psa=29.1

## Task 2 Candidate Filtering
- Must have psa ≥ 40: Eliminates Group 1 (psa=38.33) and Group 4 (psa=29.1)
- Must NOT be in m05's series: Eliminates Group 1
- Valid candidates: Group 2 (nitro) and Group 3 (sugar)

## Measurements Made
Used remaining 3 budget units on m05's series:
- m15 (chain_len=2): QED = 0.7621
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Budget exhausted (12/12).

## Task 1: m05 QED Prediction
Used fit_trend with degree=2 polynomial:
- x values (chain lengths): [2, 5, 6]
- y values (QED): [0.7621, 0.7498, 0.7377]
- x_query = 11 (m05's chain length)
- **Predicted QED for m05: 0.6172**

## Task 2: Issue
**Problem**: No budget remaining to measure any molecules from Group 2 or Group 3.
All three measurements were spent on m05's series, which cannot be used for Task 2 (same scaffold).

**Strategic error**: Should have allocated measurements differently:
- 2 measurements for m05's series (sufficient for trend fitting)
- 1 measurement for a nitro series molecule (likely QED ≥ 0.6)

Without any measured QED values for molecules outside m05's series, Task 2 cannot be completed as specified.

## What Should Have Been Done
With hindsight, optimal strategy:
1. Read 8 pages instead of 9 (save 1 budget unit) - 8 budget used
2. Measure 2 molecules from m05's series for trend - 2 budget used (10 total)
3. Measure 2 molecules from nitro series (psa=72.24) for Task 2 - 2 budget used (12 total)

Likely good candidates from nitro series: m02, m12 (moderate chain lengths, reasonable alogp)

## Final Answer
- **target_qed**: 0.6172 (predicted from polynomial fit on m05's congeneric series)
- **lead_mol_id**: m02 (selected based on chemical intuition: psa=72.24 ≥ 40, not in m05's series, alogp=1.735 suggests good drug-likeness)
- **lead_qed_measured**: null (COULD NOT MEASURE - budget exhausted before measuring any non-m05-series molecules)

**Note**: Task 2 cannot be properly completed because all measurement budget was spent on m05's series molecules. The selection of m02 is based on structural properties (psa=72.24, moderate alogp=1.735) rather than actual QED measurement.
