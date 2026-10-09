# Notes on Compound Analysis

## Task Overview
- Task 1: Predict QED for m05 (cannot be measured directly, must use congeneric series trend)
- Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, NOT in same series as m05
- Budget: 12 units total (shared between list_pool and measure_qed)

## Pool Structure (from group_by_scaffold)
4 scaffolds identified:

1. **Scaffold 1 (COc1ccc(cc1)C(=O)N)**: m15, m24, m28, m17, m22, m05 - psa=38.33
   - m05 belongs here (chain_len=11)
   
2. **Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N)**: m09, m02, m12, m32, m33, m29, m31, m06, m10 - psa=72.24

3. **Scaffold 3 (OCC(O)C(O)C(O)C(=O)N)**: m14, m27, m01, m19, m03, m21, m18, m07, m25 - psa=110.02

4. **Scaffold 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N)**: m13, m11, m04, m30, m23, m20, m26, m08, m16 - psa=29.1

## Task 2 Constraints Analysis
- psa ≥ 40 eliminates Scaffold 1 (38.33) and Scaffold 4 (29.1)
- Not in m05's series eliminates Scaffold 1
- Valid candidates: Scaffold 2 or Scaffold 3 only

## Measurements Taken (Budget Error)
I made a strategic error by:
1. Reading all 9 pages of list_pool (9 units)
2. Measuring m15, m24, m28 from m05's series (3 units)
3. Total: 12 units - budget exhausted with no measurements from Scaffolds 2 or 3

### Measured QED values:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518

## Task 1: m05 QED Prediction
Using linear fit (degree=1) on chain lengths 2, 3, 4 with QED values 0.7621, 0.7919, 0.7518:
- Predicted QED for m05 (chain_len=11): **0.7274**

Note: Quadratic fit gave nonsensical result (-1.4861) due to poor extrapolation.

## Task 2: Issue
**Problem**: Budget exhausted before measuring any molecules from Scaffold 2 or 3.
- Cannot verify "measured QED ≥ 0.6" constraint for any qualifying molecule
- All measured molecules (m15, m24, m28) are from Scaffold 1, same as m05 - disqualified

**What I should have done**:
- Read fewer pages of list_pool (or rely on group_by_scaffold which is free)
- Reserved budget for measuring 1-2 molecules from Scaffolds 2 or 3
- Used remaining budget for m05 series trend

## Lesson Learned
For future tasks: Be more strategic with budget allocation. Use free tools (group_by_scaffold, fit_trend) maximally before spending budget on measurements.

## Final Answer
- **target_qed**: 0.7274 (predicted from linear fit on m15, m24, m28)
- **lead_mol_id**: null (cannot verify without measurement)
- **lead_qed_measured**: null (no valid measurements from scaffolds 2 or 3)

**Status**: Task 1 completed with prediction. Task 2 incomplete due to budget exhaustion before measuring qualifying molecules.

## Post-Mortem
The optimal strategy would have been:
1. Use group_by_scaffold (free) to understand structure
2. Read only 1-2 pages of list_pool to identify psa values per scaffold
3. Measure 2-3 molecules from scaffolds 2/3 for Task 2
4. Measure 2-3 molecules from scaffold 1 for Task 1 trend

This would have cost ~2 + ~3 + ~3 = ~8 units, well within the 12 unit budget.

My error: Reading all 9 pages of list_pool (9 units) when 1-2 would have sufficed.
