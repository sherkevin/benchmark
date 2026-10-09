# Task Notes

## Overview
- Need to find the QED of m05 (which cannot be measured directly)
- Need to find a molecule that meets all three criteria:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. not in the same congeneric series as m05

## Step 1: Explore the compound pool
First, I'll use the group_by_scaffold tool to understand the structure of the compounds and identify which ones are in the same series as m05.

Results:
- Group 1: scaffold "COc1ccc(cc1)C(=O)N" contains m05 (chain_len: 11) and other molecules with chain lengths 2-6
- Group 2: scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N"
- Group 3: scaffold "OCC(O)C(O)C(O)C(=O)N"
- Group 4: scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N"

## Step 2: Get QED values for m05's congeneric series
Since m05 is in the same series as m15, m24, m28, m17, and m22, I'll measure their QED values to predict m05's QED.

QED values for the series:
- m15 (chain_len: 2): 0.7621
- m24 (chain_len: 3): 0.7919
- m28 (chain_len: 4): 0.7518
- m17 (chain_len: 5): 0.7498
- m22 (chain_len: 6): 0.7377

## Step 3: Predict m05's QED
Using the chain lengths and corresponding QED values to predict m05's QED (chain_len: 11)

Result: Predicted QED for m05 = 0.5416

## Step 4: Find a suitable molecule for Task 2
I need to find a molecule from groups 2, 3, or 4 that meets:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in the same congeneric series as m05

After testing various compounds:
- m01 (Group 3): QED = 0.3187 (< 0.6, doesn't meet criteria)
- m03 (Group 3): QED = 0.3282 (< 0.6, doesn't meet criteria)
- m02 (Group 2): QED = 0.6046, PSA = 72.24, not in same series as m05
  - Meets all three criteria!

## Final Results
Task 1: QED of m05 = 0.5416
Task 2: Selected m02 with measured QED = 0.6046