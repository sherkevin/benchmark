# Notes for Medicinal Chemistry Task

## Task Overview

We need to:
1. Predict QED for m05 (never synthesized, so cannot be measured directly)
2. Find one molecule that satisfies all three constraints:
   - Measured QED ≥ 0.6
   - PSA ≥ 40
   - Not in same congeneric series as m05 (different scaffold or not just N-alkyl chain length difference)

Budget: 12 QED measurements total for both tasks.

## Initial Analysis

First, let me understand the structure of the molecules by looking at scaffolds. Let me use the group_by_scaffold tool.

## Scaffold Groups Identified

There are 4 scaffold groups:

1. **COc1ccc(cc1)C(=O)N** - Methoxybenzamide series (contains m05)
   - m15 (chain_len: 2)
   - m24 (chain_len: 3)
   - m28 (chain_len: 4)
   - m17 (chain_len: 5)
   - m22 (chain_len: 6)
   - m05 (chain_len: 11) - TARGET MOLECULE

2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** - Nitrobenzamide series
   - m09 (chain_len: 2)
   - m02 (chain_len: 3)
   - m12 (chain_len: 4)
   - m32 (chain_len: 5)
   - m33 (chain_len: 6)
   - m29 (chain_len: 7)
   - m31 (chain_len: 8)
   - m06 (chain_len: 9)
   - m10 (chain_len: 10)

3. **OCC(O)C(O)C(O)C(=O)N** - Sugar-like benzamide series
   - m14 (chain_len: 2)
   - m27 (chain_len: 3)
   - m01 (chain_len: 4)
   - m19 (chain_len: 5)
   - m03 (chain_len: 6)
   - m21 (chain_len: 7)
   - m18 (chain_len: 8)
   - m07 (chain_len: 9)
   - m25 (chain_len: 10)

4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** - Biphenyl benzamide series
   - m13 (chain_len: 2)
   - m11 (chain_len: 3)
   - m04 (chain_len: 4)
   - m30 (chain_len: 5)
   - m23 (chain_len: 6)
   - m20 (chain_len: 7)
   - m26 (chain_len: 8)
   - m08 (chain_len: 9)
   - m16 (chain_len: 10)

For Task 2, we need a molecule NOT in the same series as m05, so we need to exclude group 1 (methoxybenzamide series).

## Task 1: Predicting QED for m05

### Measurements taken (Group 1 - m05's series):
1. m15 (chain_len: 2): QED = 0.7621
2. m24 (chain_len: 3): QED = 0.7919  
3. m28 (chain_len: 4): QED = 0.7518
4. m17 (chain_len: 5): QED = 0.7498
5. m22 (chain_len: 6): QED = 0.7377

Measurements used: 5, Budget remaining: 7

m05 has chain_len: 11.

### Trend Analysis:
Using xs = [2, 3, 4, 5, 6], ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]:
- Quadratic fit (degree=2) at x_query=11: QED = 0.5416 (RMSE=0.01188)
- Linear fit (degree=1) at x_query=11: QED = 0.6950 (RMSE=0.01308)

Quadratic fit has slightly better RMSE and captures the decreasing trend after chain_len 3. The predicted QED for m05 is likely around 0.54-0.55 range.

## Task 2: Finding a molecule satisfying all constraints

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40  
3. Not in same series as m05 (different scaffold)

### Candidate Search:

**Group 2 (Nitrobenzamide series, PSA = 72.24 ≥ 40):**
- m09: QED = 0.5826 (< 0.6) ❌
- m02: QED = 0.6046 (≥ 0.6) ✅
- m12: QED = 0.4709 (< 0.6) ❌
- m32: QED = 0.4683 (< 0.6) ❌

**Group 3 (Sugar-like series, PSA = 110.02 ≥ 40):**
- m14: QED = 0.3237 (< 0.6) ❌
- m01: QED = 0.3187 (< 0.6) ❌
- m19: QED = 0.3244 (< 0.6) ❌

**Group 4 (Biphenyl series, PSA = 29.1 < 40):** Excluded due to PSA constraint.

### Result:
m02 satisfies all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✅
2. PSA = 72.24 ≥ 40 ✅  
3. Not in same series as m05 (different scaffold) ✅

Total measurements used: 12/12

## Final Answer

### Task 1: Predicted QED for m05
Using quadratic trend fitting based on 5 measured molecules from same scaffold series (chain lengths 2-6):
- Predicted QED for m05 (chain length 11): **0.5416**

### Task 2: Selected molecule satisfying all constraints
**m02** satisfies:
1. Measured QED = **0.6046** ≥ 0.6
2. PSA = 72.24 ≥ 40
3. Different scaffold from m05 (nitrobenzamide vs methoxybenzamide)

### Deliverables submitted to answer.json:
```json
{
 "target_qed": 0.5416,
 "lead_mol_id": "m02",
 "lead_qed_measured": 0.6046
}
```