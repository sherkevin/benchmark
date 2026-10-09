# Notes

## Task Overview
- Task 1: Predict QED of m05 (not synthesizable, must use trend from congeneric series)
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's series
- Budget: 12 units total (shared between list_pool and measure_qed)

## Strategy Execution (with error)
I made a strategic error in budget allocation:

1. **list_pool**: Used 9 units to read all 9 pages (33 molecules)
2. **group_by_scaffold**: Free - identified 4 scaffolds
   - m05's series: COc1ccc(cc1)C(=O)N with members m15, m24, m28, m17, m22, m05
   - Other series with psa ≥ 40: O=[N+]([O-])c1ccc(cc1)C(=O)N (psa=72.24) and OCC(O)C(O)C(O)C(=O)N (psa=110.02)
3. **measure_qed**: Used 3 units on m15, m24, m28 (all in m05's series)

**Error**: I spent all 12 budget units (9 + 3) on reading the pool and measuring molecules from m05's series. I did not save any budget for Task 2 measurements.

## Measurements Made
- m15: QED = 0.7621 (chain_len=2, psa=38.33, in m05 series)
- m24: QED = 0.7919 (chain_len=3, psa=38.33, in m05 series)
- m28: QED = 0.7518 (chain_len=4, psa=38.33, in m05 series)

## Task 1: m05 QED Prediction
Using fit_trend with linear fit (degree=1) on the three measurements:
- xs = [2, 3, 4] (chain lengths)
- ys = [0.7621, 0.7919, 0.7518] (QED values)
- x_query = 11 (m05's chain length)

Result: predicted QED = 0.7274

The quadratic fit gave -1.4861 (non-physical), so linear fit is more appropriate for extrapolation.

## Task 2: Issue
I have no valid measurements for Task 2 candidates. All my measurements (m15, m24, m28) are:
1. In m05's congeneric series (fail condition 3)
2. Have psa = 38.33 < 40 (fail condition 2)

Budget is exhausted (12/12 used), so I cannot measure any Task 2 candidates.

**Correct strategy would have been**:
- Read fewer pages (e.g., 6 pages = 6 units)
- Measure 2 molecules from m05 series for trend (2 units)
- Measure 1-2 molecules from other series with psa ≥ 40 (2-3 units)
- Total: ~10-11 units, leaving buffer

## Conclusion
Task 1 completed with predicted QED of 0.7274 for m05 (using linear fit on m15, m24, m28).
Task 2 CANNOT be completed due to budget exhaustion - I have no valid measurement for any molecule that satisfies conditions 2 (psa ≥ 40) and 3 (not in m05 series).

**Final answer submitted** (in answer.json):
- target_qed: 0.7274 (predicted for m05 using linear fit)
- lead_mol_id: "m01" (has psa=110.02 ≥ 40, not in m05 series, but QED NOT measured)
- lead_qed_measured: null (could not measure due to budget exhaustion)

This answer is INCOMPLETE because:
1. I cannot verify that m01 has QED ≥ 0.6 without a measurement.
2. The task requires lead_qed_measured to be a number from a real measure_qed call, but I have none for a valid Task 2 candidate.

**Root cause**: Budget mismanagement - spent 9 units on list_pool (reading all 9 pages) and 3 units on m05-series measurements, leaving 0 units for Task 2.

**Lesson learned**: Budget allocation should have been:
- 1-2 pages to identify Task 2 candidates (not 9)
- 2-3 measurements for m05 series trend
- 1-2 measurements for Task 2 candidates
Total: ~4-7 units, leaving buffer.
