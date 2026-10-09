# Task Notes

## Overview
I need to complete two tasks:
1. Report QED for m05 (which has never been synthesized, so I need to predict it)
2. Pick one molecule that satisfies all three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in the same congeneric series as m05

I have 12 measurement budget total for both tasks.

## Step 1: Understanding the data
First, let me understand the molecules and identify scaffolds. I'll use the `group_by_scaffold()` tool to see how molecules are grouped by scaffold.

Scaffold groups found:
1. "COc1ccc(cc1)C(=O)N" - includes m05 (target, chain_len=11) and others with chain_len 2-6
2. "O=[N+]([O-])c1ccc(cc1)C(=O)N" - nitrobenzene scaffold
3. "OCC(O)C(O)C(O)C(=O)N" - sugar-like scaffold  
4. "c1ccc(cc1)-c1ccc(cc1)C(=O)N" - biphenyl scaffold

For Task 1: Need to predict m05's QED by measuring other molecules in scaffold group 1 and fitting trend.

For Task 2: Need to find molecule NOT in scaffold group 1, with psa ≥ 40 and measured QED ≥ 0.6.

## Step 2: Task 1 - Predicting m05's QED
m05 is in scaffold group 1 with chain_len=11. Other members: m15(2), m24(3), m28(4), m17(5), m22(6).

Strategy: Measure QED for some of these to establish trend vs chain length, then predict m05's QED using fit_trend.

Budget: 12 total measurements for both tasks.

**Measurements for scaffold group 1 (Task 1):**
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919  
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Observations: QED seems to decrease as chain length increases beyond 3.

**Task 1 prediction for m05 (chain_len=11):**
- Linear fit (degree=1): QED = 0.695
- Quadratic fit (degree=2): QED = 0.5416 (lower RMSE: 0.01188 vs 0.01308)
- Cubic fit (degree=3): QED = 2.1323 (invalid, > 1)

I'll use quadratic fit prediction: 0.5416

**Remaining budget:** 7 measurements (12 total - 5 used)

## Step 4: Task 2 - Finding suitable molecule
Measured candidates from scaffold group 2 (nitrobenzene, psa=72.24):
- m02 (chain_len=3): QED = 0.6046 ✓ (≥ 0.6)
- m12 (chain_len=4): QED = 0.4709
- m32 (chain_len=5): QED = 0.4683  
- m09 (chain_len=2): QED = 0.5826

Measured candidates from scaffold group 3 (sugar-like, psa=110.02):
- m14 (chain_len=2): QED = 0.3237
- m27 (chain_len=3): QED = 0.333
- m01 (chain_len=4): QED = 0.3187

**Best candidate found:** m02 with QED = 0.6046
- Satisfies: QED ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓  
- Not in same scaffold as m05 ✓ (different scaffold group)

**Budget used:** 12/12 measurements

## Step 5: Final Results
Task 1: Predicted QED for m05 = 0.5416 (using quadratic fit on measured QED values from scaffold group 1 members: m15, m24, m28, m17, m22)

Task 2: Selected m02 as lead molecule with:
- Measured QED = 0.6046 (≥ 0.6)
- psa = 72.24 (≥ 40)
- Different scaffold from m05 (scaffold group 2 vs group 1)

**QED components analysis:**
- m02: ALERTS score = 0.2414 pulls down overall QED; other scores high (0.83-0.99)
- m22 (from m05's scaffold): ROTB = 0.5332 lower due to longer chain; ALERTS = 0.6397

**All measurements used:** m15, m24, m28, m17, m22, m02, m12, m32, m09, m14, m27, m01 (12/12)

## Step 3: Task 2 - Finding candidate molecules
Candidates must have psa ≥ 40 and NOT be in scaffold group 1.
Scaffold groups:
- Group 2 (nitrobenzene): psa = 72.24 for all members
- Group 3 (sugar-like): psa = 110.02 for all members  
- Group 4 (biphenyl): psa = 29.1 for all members (< 40, so excluded)

So candidates are only from groups 2 and 3.