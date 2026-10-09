# Task Notes

## Task Overview
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) which has never been synthesized
2. Find a molecule from the pool that meets all three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in the same congeneric series as m05

Total budget: 17 units shared between list_pool and measure_qed operations.

## Strategy
1. First, understand the pool structure using group_by_scaffold()
2. Identify which scaffold m05 belongs to
3. Measure QED for other molecules in m05's series to establish trend
4. Use fit_trend to predict QED for m05
5. Search for molecules meeting Task 2 criteria while being efficient with budget

## Step xxx: Initial exploration
Let's start by using the free tools to understand the data.

## Step 2: Scaffold Analysis
From `group_by_scaffold()`:
- 4 scaffold groups identified
- m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain length 11
- Other members in this series: m15 (len 2), m24 (len 3), m28 (len 4), m17 (len 5), m22 (len 6)
- Need to measure QED for some of these to establish trend for m05

Total molecules in pool: 33 molecules (6 in m05's series, 9 in each of the other 3 series)

## Step 3: Plan for Task 1 (predict m05 QED)
1. Measure QED for some molecules in m05's series
2. Use fit_trend to predict m05's QED at chain length 11
3. Need to be efficient with budget (17 total units)

## Step 4: Data Collection
Budget used so far: 7 units (pages 1-7)
Budget remaining: 10 units

Molecules in m05's series (scaffold `COc1ccc(cc1)C(=O)N`):
- m15: chain length 2
- m24: chain length 3  
- m28: chain length 4
- m17: chain length 5
- m22: chain length 6
- m05: chain length 11 (target, not synthesized)

Need to measure QED for some of these to establish trend.

## Step 5: QED Measurements for m05 Series
Measured QED values:
- m15 (chain length 2): 0.7621
- m24 (chain length 3): 0.7919  
- m28 (chain length 4): 0.7518
- m17 (chain length 5): 0.7498
- m22 (chain length 6): 0.7377

Budget used: 12 units (7 pages + 5 measurements)
Budget remaining: 5 units

Pattern: QED seems to decrease as chain length increases beyond length 3.

## Step 6: Task 2 Candidate Search
Budget remaining after Task 1 measurements: 5 units

Criteria for Task 2:
1. measured QED ≥ 0.6
2. psa ≥ 40  
3. not in same series as m05 (different scaffold)

Scaffolds with psa ≥ 40:
- Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): psa = 72.24
- Scaffold 3 (OCC(O)C(O)C(O)C(=O)N): psa = 110.02

Measured candidates:
- m02 (scaffold 2, chain length 3): QED = 0.6046 ✓ meets all criteria
- m01 (scaffold 3, chain length 4): QED = 0.3187 ✗ fails QED ≥ 0.6
- m12 (scaffold 2, chain length 4): QED = 0.4709 ✗ fails QED ≥ 0.6

Best candidate: m02 with QED 0.6046

Budget usage summary:
- Pages 1-9: 9 units
- QED measurements: 8 units (m15, m24, m28, m17, m22, m02, m01, m12)
- Total: 17/17 units used

## Final Results
Task 1: Predicted QED for m05 (chain length 11) = 0.6972 (using quadratic fit on all 5 measurements)
Task 2: Selected m02 with measured QED = 0.6046
- m02 has psa = 72.24 (≥ 40)
- m02 is from scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N), different from m05's scaffold
- m02 QED = 0.6046 (≥ 0.6)

All budget used efficiently.