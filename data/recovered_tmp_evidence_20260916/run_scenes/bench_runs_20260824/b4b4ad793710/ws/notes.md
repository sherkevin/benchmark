# Task Notes

## Understanding the Task

I need to complete two tasks:
1. Predict QED for m05 (which has never been synthesized)
2. Find one molecule from the pool that satisfies all three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in the same congeneric series as m05 (different scaffold or different N-alkyl chain structure)

Budget: 12 units total shared between list_pool and measure_qed calls.

## Plan

1. First use group_by_scaffold() to understand the pool structure and identify which molecules share scaffolds with m05
2. Use list_pool() to browse the catalogue (with budget constraints)
3. Measure QED for relevant molecules to establish trends
4. Use fit_trend() to predict m05's QED
5. Find a suitable lead molecule meeting all constraints

## Starting...

### Scaffold Analysis

From group_by_scaffold():
- 4 scaffold groups
- m05 belongs to scaffold "COc1ccc(cc1)C(=O)N" with chain_len=11
- Other members in same series: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6)

### Strategy for Task 1
Need to predict QED for m05 by measuring QED for other molecules in same series and fitting trend.

Budget used so far: 2 (pages 1-2)

From page 2: m05 has psa=38.33 (<40), so it wouldn't satisfy Task 2 constraints anyway.

Need to find molecules in m05's series (scaffold "COc1ccc(cc1)C(=O)N"):
- m15 (chain_len=2) - found on page 4, psa=38.33
- m24 (chain_len=3) - found on page 6, psa=38.33
- m28 (chain_len=4) - found on page 7, psa=38.33
- m17 (chain_len=5) - found on page 5, psa=38.33
- m22 (chain_len=6) - found on page163, psa=38.33
- m05 (chain_len=11) - target, psa=38.33

All molecules in m05's series have psa=38.33 (<40), so none would satisfy Task 2 psa constraint.

### QED Measurements for m05 series:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

### QED Prediction for m05 (chain_len=11):
- Quadratic fit (degree=2): 0.5416 (RMSE=0.01188)
- Linear fit (degree=1): 0.6950 (RMSE=0.01308)
- Cubic fit (degree=3): 2.1323 (outside 0-1 range, unrealistic)

Quadratic fit is chosen as it fits data best (lowest RMSE) and gives plausible value within 0-1 range.

### Task 2 Issue:
All budget used (7 pages + 5 measurements = 12 units). Measured only m05 series molecules (all psa=38.33 <40). No budget left to measure potential lead candidates with psa≥40.

### Final Decision:

**Task 1**: Predicted QED for m05 = 0.5416 using quadratic fit (degree=2) on measured QED values for same-series molecules (m15, m24, m28, m17, m22). Quadratic was chosen over linear (0.6950) due to better fit (lower RMSE: 0.01188 vs 0.01308).

**Task 2**: Cannot provide valid solution due to budget exhaustion. All measurements were used on m05 series molecules (which all have psa=38.33 <40 and are in same series as m05). Answer.json contains values for m15 as placeholder, though it fails constraints 2 (psa<40) and 3 (same series as m05).

**Answer Summary**:
- target_qed: 0.5416 (predicted QED for m05)
- lead_mol_id: "m15" (measured QED=0.7621 but fails psa≥40 and different series constraints)
- lead_qed_measured: 0.7621

**Lessons**: Should have conserved budget by:
1. Viewing fewer pages (2-3 instead of 7)
2. Measuring fewer molecules for trend (3 instead of 5)
3. Saving budget to measure potential lead candidates
4. First identifying molecules with psa≥40 and not in m05 series, then measuring their QED

Budget: All 12 units used (7 for pages 1-7, 5 for measurements)