# Task Notes

## Understanding the Task

We have a pool of 33 molecules with:
- `mol_id`, `smiles`, and five descriptors: `mw`, `alogp`, `psa`, `rotb`, `arom`
- Need to use `list_pool` to read the catalogue (4 entries per page, 1 budget unit per page)
- Can measure QED with `measure_qed` (1 budget unit per measurement)
- Total budget: 12 units shared between list_pool and measure_qed

## Task 1: Predict QED for m05
- m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- It's never been synthesized, so `measure_qed` returns `probe_unavailable`
- Need to predict its QED using congeneric series (same scaffold, only N-alkyl chain length differs)
- Can use `fit_trend` for polynomial fitting

## Task 2: Find a molecule that satisfies:
1. measured QED ≥ 0.6
2. `psa` ≥ 40
3. not in the same congeneric series as m05

## Approach
1. First, get overview of the pool with `group_by_scaffold()` to understand series
2. Identify which series m05 belongs to and its N-alkyl chain length
3. Measure QED for other molecules in same series to establish trend
4. Use `fit_trend` to predict m05's QED
5. Search for candidate molecules meeting Task 2 criteria
6. Measure QED of promising candidates to confirm

## Scaffold Groups (from group_by_scaffold)
1. Scaffold: `COc1ccc(cc1)C(=O)N` (m05's series)
   - m15 (chain_len=2): QED 0.7621 (measured)
   - m24 (chain_len=3): QED 0.7919 (measured)
   - m28 (chain_len=4): QED 0.7518 (measured)
   - m17 (chain_len=5): QED 0.7498 (measured)
   - m22 (chain_len=6): QED 0.7377 (measured)
   - m05 (chain_len=11) - target molecule (needs prediction)

2. Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)

3. Scaffold: `OCC(O)C(O)C(O)C(=O)N`
   - m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)

4. Scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
   - m13 (2), m11 (3), m04 (4), m30 (5), m23 (6), m20 (7), m26 (8), m08 (9), m16 (10)

## Budget Considerations
- 12 total budget units
- Need to be efficient with list_pool pages
- Each measurement costs 1 unit
- Pool has 33 molecules, list_pool shows 4 per page = 9 pages needed to see all (9 budget units)

## Budget Used So Far
- 5 measurements for m05 series: m22, m17, m28, m24, m15 (5 units)
- 3 measurements for Task 2 candidates: m01, m02, m06 (3 units)
- 4 list_pool pages: page 1, 2, 3, 4 (4 units)
- Total used: 12 units
- Remaining: 0 units

## Task 1 Progress
- Measured QED for all molecules in m05's series except m05:
  - m15 (chain_len=2): QED 0.7621
  - m24 (chain_len=3): QED 0.7919
  - m28 (chain_len=4): QED 0.7518
  - m17 (chain_len=5): QED 0.7498
  - m22 (chain_len=6): QED 0.7377
- Quadratic fit predicts m05 QED ≈ 0.5416
- Linear fit predicts m05 QED ≈ 0.695
- Quadratic seems more reasonable given downward trend

## Task 2 Potential Candidates (psa ≥ 40, not m05 series)
From pages 1-3:
- m01: psa=110.02 (scaffold group 3) - measured QED 0.3187 < 0.6
- m02: psa=72.24 (scaffold group 2) - measured QED 0.6046 ≥ 0.6 ✓
- m03: psa=110.02 (scaffold group 3) - not measured (no budget)
- m06: psa=72.24 (scaffold group 2) - measured QED 0.4011 < 0.6
- m07: psa=110.02 (scaffold group 3) - not measured (no budget)
- m09: psa=72.24 (scaffold group 2) - not measured (no budget)
- m10: psa=72.24 (scaffold group 2) - not measured (no budget)
- m12: psa=72.24 (scaffold group 2) - not measured (no budget)

## Final Decisions

### Task 1: m05 QED Prediction
Using quadratic fit (degree=2) on measured QED values for m05's series:
- xs: [2, 3, 4, 5, 6] (chain lengths)
- ys: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377] (measured QED)
- x_query: 11 (m05 chain length)
- Prediction: 0.5416

The quadratic fit is chosen over linear because:
1. The data shows a clear downward trend (0.7919 → 0.7377)
2. Quadratic has lower RMSE (0.01188 vs 0.01308)
3. Extrapolating to chain_len=11, quadratic gives more conservative/reasonable estimate

### Task 2: Lead Molecule Selection
Selected: m02
Reasons:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in same series as m05: scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N` vs m05's scaffold `COc1ccc(cc1)C(=O)N` ✓
4. Confirmed by measurement (budget available)