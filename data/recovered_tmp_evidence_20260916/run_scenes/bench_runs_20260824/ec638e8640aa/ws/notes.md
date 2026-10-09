# Notes

## Task Overview
- Task 1: Predict QED for m05 (never synthesized, probe_unavailable)
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's scaffold group
- Budget: 12 units total (shared between list_pool and measure_qed)

## Data Collection
Read all 9 pages of the compound pool (33 molecules total). Budget used: 9 units.

## Scaffold Groups (from free group_by_scaffold call)
1. **COc1ccc(cc1)C(=O)N** (m05's group): m15, m24, m28, m17, m22, m05 - psa=38.33
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09, m02, m12, m32, m33, m29, m31, m06, m10 - psa=72.24
3. **OCC(O)C(O)C(O)C(=O)N**: m14, m27, m01, m19, m03, m21, m18, m07, m25 - psa=110.02
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13, m11, m04, m30, m23, m20, m26, m08, m16 - psa=29.1

## Task 2 Candidate Analysis
- Constraint: psa ≥ 40 eliminates Group 1 (38.33) and Group 4 (29.1)
- Constraint: not in m05's scaffold eliminates Group 1
- Valid candidates: Group 2 (nitro) and Group 3 (sugar)

## Measurements Made
Spent remaining 3 budget units on m05's scaffold group:
- m15 (chain_len=2): QED = 0.7621
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

## Task 1: Predicting m05's QED
m05 has chain_len=11 in its scaffold group.

Used fit_trend with degree=2 polynomial:
- xs: [2, 5, 6] (chain lengths)
- ys: [0.7621, 0.7498, 0.7377] (QED values)
- x_query: 11 (m05's chain length)

**Result: predicted QED = 0.6172**

## Task 2: Problem
Budget exhausted (12/12) before measuring any valid Task 2 candidates.
- All measurements (m15, m17, m22) are in m05's scaffold group - invalid for Task 2
- No measurements from Group 2 or Group 3 (the valid candidate groups)
- Cannot verify any molecule satisfies QED ≥ 0.6 constraint

## Strategic Error
I should have:
1. Reserved budget for Task 2 measurements first
2. Measured 1-2 molecules from Groups 2/3 to find QED ≥ 0.6 candidates
3. Then used remaining budget for m05's scaffold trend fitting

Instead, I spent all measurements on m05's scaffold, leaving no budget for Task 2 verification.

## Conclusion
- Task 1: Completed - predicted QED = 0.6172 (using degree=2 polynomial fit)
- Task 2: Cannot complete - no valid measurements from eligible candidates (budget exhausted)

## Final Answer
```json
{
  "target_qed": 0.6172,
  "lead_mol_id": null,
  "lead_qed_measured": null
}
```

Note: The null values for Task 2 reflect budget exhaustion before measuring any valid candidates. A better strategy would have been to measure Task 2 candidates (Groups 2 or 3) first, then use remaining budget for Task 1 trend fitting.
