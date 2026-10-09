# Task Notes
Date: 2026-08-22

## Overview
I need to complete two tasks:
1. Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - never synthesized, must predict from congeneric series
2. Find a molecule meeting: QED ≥ 0.6, PSA ≥ 40, not in same series as m05

Budget: 12 units total shared between list_pool and measure_qed.

## Plan
1. First explore the pool structure using list_pool to understand available molecules
2. Identify congeneric series related to m05 (same scaffold, only N-alkyl chain length differs)
3. Measure QED for members of m05's series to establish trend
4. Predict QED for m05 based on trend
5. Search for candidate molecule meeting all constraints
6. Measure its QED to confirm

## Step 1: Initial Pool Exploration

Page 1 (budget used: 1/12):
- m01: SMILES: OCC(O)C(O)C(O)C(=O)NCCCC, PSA: 110.02
- m02: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC, PSA: 72.24
- m03: SMILES: OCC(O)C(O)C(O)C(=O)NCCCCCC, PSA: 110.02
- m04: SMILES: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC, PSA: 29.1

Page一百 3 (budget used: 3/12):
- m09: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCC, PSA: 72.24
- m10: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCCC, PSA: 72.24
- m11: SMILES: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC, PSA: 29.1
- m12: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC, PSA: 72.24

Page 8 (budget used: 8/12):
- m29: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCC, PSA: 72.24
- m30: SMILES: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCC, PSA: 29.1
- m31: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCC, PSA: 72.24
- m32: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCC, PSA: 72.24

Page 9 (budget used: 9/12):
- m33: SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCC, PSA: 72.24

## Summary of m05's Congeneric Series
All have scaffold: COc1ccc(cc1)C(=O)N...
- m15: COc1ccc(cc1)C(=O)NCC (2-carbon chain, PSA: 38.33)
- m24: COc1ccc(cc1)C(=O)NCCC (3-carbon chain, PSA: 38.33)
- m28: COc1ccc(cc1)C(=O)NCCCC (4-carbon chain, PSA: 38.33)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5-carbon chain, PSA: 38.33)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6-carbon chain, PSA: 38.33)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (13-carbon chain, PSA: 38.33)

All have PSA = 38.33. Need to measure QED for some of these to predict m05's QED.

## QED Measurements (Task 1)

Measured (budget used: 12/12):
m15 (2-carbon chain) QED = 0.7621
m17 (5-carbon chain) QED = 0.7498
m22 (6-carbon chain) QED = 0.7377

Chain length vs QED:
- 2C: 0.7621
- 5C: 0.7498
- 6C: 0.7377

Target m05 has 13-carbon chain. QED seems to decrease with increasing chain length.

## Task 1: Predicting m05 QED

Using linear trend from measurements:
- Slope ≈ -0.0041 per carbon (from 2C to 5C) and -0.0121 per carbon (from 5C to 6C)
- Average slope ≈ -0.0081 per carbon
- From 6C to 13C: 7 carbons → ΔQED ≈ -0.0567
- Predicted QED for m05 (13C): 0.7377 - 0.0567 = 0.6810 ≈ 0.68

## Task 2: Finding Candidate

Constraints:
1. QED ≥ 0.6 (must measure)
2. PSA ≥ 40
3. Not in same series as m05

Candidate: m01
- PSA = 110.02 ≥ 40 ✓
- Not in m05's series (different scaffold: OCC(O)C(O)C(O)C(=O)NCCCC vs COc1ccc(cc1)C(=O)N...) ✓
- QED unknown (cannot measure due to budget exhaustion)

Budget issue: Used all 12 units (9 list_pool + 3 measure_qed). Should have conserved budget for Task 2 measurement.

## Final Decisions

1. Task 1: Predicted m05 QED = 0.68 (based on linear trend from m15, m17, m22 measurements)
2. Task 2: Selected m01 as candidate (PSA = 110.02 ≥ 40, not in m05 series)
   - Cannot measure QED due to budget exhaustion
   - Using placeholder value 0.0 for lead_qed_measured (not a real measurement)

Lessons learned: Budget management is critical. Should have:
1. Used fewer list_pool pages: Could have stopped after page 7 (found m05 series members: m15, m17, m22, m24, m28)
2. Used only 2 measurements for Task 1 trend (e.g., m15 and m22), saving 1 unit
3. With saved unit, measure QED for Task 2 candidate (e.g., m01 or m02)
4. Total budget: list_pool (7) + measure_qed Task 1 (2) + measure_qed Task 2 (1) = 10 units, within budget

Optimal strategy:
- Task 1: Measure shortest (m15) and longest available (m22) in series, extrapolate to m05
- Task 2: Pick molecule with PSA ≥ 40 early (many options), measure its QED