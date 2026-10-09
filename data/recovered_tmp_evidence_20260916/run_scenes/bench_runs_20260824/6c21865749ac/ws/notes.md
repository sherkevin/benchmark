# Task Notes

## Overview
- There are 33 molecules in the pool
- Need to complete two tasks:
  1. Predict QED of m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - never synthesized
  2. Find one molecule with: QED ≥ 0.6, PSA ≥ 40, and not in same series as m05

## Budget
- 12 units total, shared between list_pool and measure_qed
- list_pool: 1 unit per page (4 entries per page)
- measure_qed: 1 unit per measurement (except probe_unavailable costs 0)

## Approach

### Step 1: Understand the pool structure
Use group_by_scaffold() to understand series relationships and identify which series m05 belongs to.

### Step 2: For Task 1
- Identify which series m05 belongs to
- Measure QED of some molecules in that series to establish trend
- Use fit_trend to predict m05's QED based on chain length

### Step 3: For Task 2
- Search pool for molecules with PSA ≥ 40
- Among those, measure QED to find ones with QED ≥ 0.6
- Ensure selected molecule is not in same series as m05

## Scaffold Groups (from group_by_scaffold)
1. Group 1: Scaffold `COc1ccc(cc1)C(=O)N` - m05 belongs here (chain_len=11)
   - m15 (chain_len=2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)
   
2. Group 2: Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)
   
3. Group 3: Scaffold `OCC(O)C(O)C(O)C(=O)N`
   - m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)
   
4. Group 4: Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
   - m13 (2), m11 (3), m04 (4), m30 (5), m23 (6), m20 (7), m26 (8), m08 (9), m16 (10)

## Final Results

### Task 1: Predict QED of m05
**Data collected for Group 1 (m05's series):**
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919  
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

**Prediction using quadratic fit (degree=2):**
- Predicted QED for m05 (chain_len=11) = 0.5416
- Linear fit gave 0.695, but quadratic seems more appropriate given peak at chain_len=3

### Task 2: Find molecule with QED ≥ 0.6, PSA ≥ 40, not in same series as m05
**Selected molecule: m02**
- PSA = 72.24 (≥ 40, confirmed from list_pool page 1)
- Measured QED = 0.6046 (≥ 0.6)
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (Group 2)
- NOT in same series as m05 (Group 1 has scaffold `COc1ccc(cc1)C(=O)N`)

**Alternatives considered:**
- m01 (Group 3): QED=0.3187 < 0.6
- m12 (Group 2): QED=0.4709 < 0.6

### Budget Usage
- Total budget: 12 units
- Used: 12 units (all used)
  - list_pool: 4 pages (4 units)
  - measure_qed: 8 measurements (8 units)

## Task 2 Strategy
- Need to find molecule with PSA ≥ 40, QED ≥ 0.6, and NOT in Group 1
- Should look at other groups first
- Use list_pool to find PSA values

## Initial Plan
1. First, get some list_pool pages to understand PSA distribution
2. Measure QED for Task 1 series (Group 1 molecules)
3. Find candidate for Task 2