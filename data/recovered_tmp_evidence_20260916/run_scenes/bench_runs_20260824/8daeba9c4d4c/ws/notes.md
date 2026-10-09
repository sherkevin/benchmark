# Notes

## Task Overview
- Task 1: Report QED of m05 (cannot measure directly, must predict from congeneric series)
- Task 2: Find one molecule with QED >= 0.6, psa >= 40, not in same congeneric series as m05

## Budget
- 17 units total (shared between list_pool and measure_qed)
- list_pool: 1 unit per page (4 entries per page)
- measure_qed: 1 unit per measurement

## Plan
1. List the pool to understand the molecules
2. Use group_by_scaffold to understand congeneric series
3. Measure QED for molecules in m05's series to build a trend
4. Predict m05's QED using fit_trend
5. Find a lead molecule meeting all criteria

## Progress
- [x] List pool entries (9 pages, 9 budget units)
- [x] Group by scaffold (found 4 scaffolds)
- [x] Measure QED for m05 series (m15, m24, m28, m17, m22 - 5 measurements)
- [x] Predict m05 QED using fit_trend (degree 2 polynomial, prediction = 0.5416)
- [x] Find lead molecule (m02: QED=0.6046, psa=72.24, nitro series)
- [x] Write answer.json

## Results

### Task 1: m05 QED prediction
Measured QED for m05's congeneric series:
- m15 (chain_len=2): 0.7621
- m24 (chain_len=3): 0.7919
- m28 (chain_len=4): 0.7518
- m17 (chain_len=5): 0.7498
- m22 (chain_len=6): 0.7377

Used fit_trend with degree=2 polynomial to extrapolate to chain_len=11:
**Predicted QED for m05: 0.5416**

### Task 2: Lead molecule selection
Criteria: QED >= 0.6, psa >= 40, not in m05's series

Measurements:
- m02: QED = 0.6046, psa = 72.24 (nitro series) ✓ VALID
- m12: QED = 0.4709 (too low)
- m01: QED = 0.3187 (too low)

**Selected lead: m02** with measured QED = 0.6046

## Budget usage
- list_pool: 9 units (9 pages)
- measure_qed: 8 units (m15, m24, m28, m17, m22, m02, m12, m01)
- Total: 17/17 units
