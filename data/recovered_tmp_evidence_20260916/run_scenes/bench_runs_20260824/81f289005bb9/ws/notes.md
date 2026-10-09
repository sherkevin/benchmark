# Task Notes

## Analysis
From `group_by_scaffold()`:
- 4 scaffolds with 33 total molecules
- m05 belongs to scaffold "COc1ccc(cc1)C(=O)N" with chain length 11
- Other members in this series: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6)

## Task 1: Predicting QED for m05
Strategy: Measure QED of several compounds in m05's congeneric series, fit a trend (polynomial) using `fit_trend`, and predict QED for chain length 11.

Budget constraints: 17 units total for both `list_pool` and `measure_qed`.

## Task 2: Finding lead molecule
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40  
3. not in same congeneric series as m05 (scaffold "COc1ccc(cc1)C(=O)N")

Need to explore other scaffolds and identify candidates.

## Current Status
- Viewed all 33 compounds (9 pages, 9 budget units used)
- Used all 17 budget units (9 for list_pool, 8 for measurements)
- Compounds in m05's series all have psa = 38.33 (just below 40 threshold)

## Task 1 Results
Measured QED for m05 series compounds:
- m15 (chain_len 2): QED = 0.7621
- m24 (chain_len 3): QED = 0.7919  
- m28 (chain_len 4): QED = 0.7518
- m17 (chain_len 5): QED = 0.7498
- m22 (chain_len 6): QED = 0.7377

Trend analysis: QED peaks at chain_len 3, then gradually decreases.
Linear fit prediction for m05 (chain_len 11): QED = 0.695
Quadratic fit prediction: QED = 0.5416
Using linear fit as more reasonable: **target_qed = 0.695**

## Task 2 Results
Measured candidates:
- m02 (scaffold 2): QED = 0.6046, psa = 72.24 ✓
- m09 (scaffold 2): QED = 0.5826 (below 0.6 threshold)
- m01 (scaffold 3): QED = 0.3187 (too low)

Best candidate: **m02** with QED = 0.6046
Constraints satisfied:
1. measured QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24)
3. not in same congeneric series as m05 ✓ (scaffold 2, not scaffold 1)