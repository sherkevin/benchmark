# Task Notes

## Task 1: Predict QED for m05

### Approach
- m05 is in scaffold group 1: COc1ccc(cc1)C(=O)N with chain length 11
- Measured QED for molecules in same series:
  - m15 (chain length 2): QED = 0.7621
  - m24 (chain length 3): QED = 0.7919
  - m28 (chain length 4): QED = 0.7518
  - m17 (chain length 5): QED = 0.7498
  - m22 (chain length 6): QED = 0.7377

### Trend Analysis
- QED peaks at chain length 3 (0.7919)
- Decreases with longer chain lengths: 0.7518, 0.7498, 0.7377
- Quadratic fit (degree=2) predicts QED for chain length 11: 0.5416
- Linear fit predicts 0.695, but quadratic seems more appropriate given downward trend

### Decision
Using quadratic fit prediction: **0.5416**

## Task 2: Find molecule meeting all constraints

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in same series as m05 (different scaffold)

### Budget Status
- Total budget: 17 units
- Used so far: 14 units (9 list_pool pages + 5 measurements)
- Remaining: 3 units

### Candidate Screening
Scaffold groups:
1. Group 1 (COc1ccc(cc1)C(=O)N): Same as m05 - EXCLUDED
2. Group 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): All have PSA = 72.24 ≥ 40
3. Group 3 (OCC(O)C(O)C(O)C(=O)N): All have PSA = 110.02 ≥ 40
4. Group 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N): All have PSA = 29.1 < 40 - EXCLUDED

### Strategy
Need to measure QED for candidates from groups 2 or 3 to find one with QED ≥ 0.6.
With 3 budget units remaining, can measure up to 3 molecules.

### Measurements
1. m02 (scaffold group 2, chain length 3): QED = 0.6046 ✓ Meets all constraints
2. m09 (scaffold group 2, chain length 2): QED = 0.5826 < 0.6
3. m01 (scaffold group 3, chain length 4): QED = 0.3187 < 0.6

### Selected Candidate
**m02** satisfies all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Different scaffold from m05 (nitrobenzene vs methoxybenzene) ✓

Budget used all 17 units.