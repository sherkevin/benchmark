# Task Notes

## Overview
- 33 molecules total, 4 scaffolds
- m05 is in scaffold 1 with chain length 11
- Need to predict m05 QED by fitting trend from its series (scaffold 1)
- Need to find molecule with QED ≥ 0.6, psa ≥ 40, not in scaffold 1
- Budget: 12 units total, used 9 so far (for viewing all pages), remaining: 3

## Key Findings from Pool Scan
- Scaffold 1 (m05 series): all have psa = 38.33 (just below 40)
- Scaffold 2: all have psa = 72.24 (above 40)
- Scaffold 3: all have psa = 110.02 (above 40) 
- Scaffold 4: all have psa = 29.1 (below 40)

## Plan
1. First gather data on scaffold 1 members to predict m05 QED
2. Search pool for molecules meeting criteria, focusing on scaffolds 2 and 3 (psa ≥ 40)
3. Measure QEDs of promising candidates until we find one meeting all criteria

## Scaffold 1 members (m05 series):
- m15 (chain_len: 2, psa: 38.33) - QED: 0.7621
- m24 (chain_len: 3, psa: 38.33) - QED: 0.7919
- m28 (chain_len: 4, psa: 38.33) - QED: 0.7518
- m17 (chain_len: 5, psa: 38.33)
- m22 (chain_len: 6, psa: 38.33)
- m05 (chain_len: 11, psa: 38.33) - target

## Budget Tracking
Budget used: 12/12 (all spent)
- 9 units for viewing all pages (9 pages)
- 3 units for measuring QEDs: m15, m24, m28

## m05 QED Prediction
Using chain lengths [2, 3, 4] and QEDs [0.7621, 0.7919, 0.7518]:
- Linear fit (degree 1) predicts QED = 0.7274 for chain_len = 11
- Quadratic fit (degree 2) predicts QED = -1.4861 (unreasonable)

Will use linear prediction: 0.7274

## Problem Analysis
I've used all 12 budget units but haven't completed Task 2. Need to find a molecule meeting all 3 criteria but have no budget left to measure QEDs.

## Strategic Error
1. Used 9/12 budget on viewing all pages - excessive
2. Used remaining 3 budget on scaffold 1 measurements for Task 1
3. Left 0 budget for Task 2 measurements
4. Should have allocated budget more strategically: fewer page views, saved budget for scaffold 2/3 measurements

## Task 1 Completed
- Predicted m05 QED = 0.7274 using linear fit on m15, m24, m28 measurements

## Task 2 Cannot Be Completed
- Need to report measured QED for a molecule meeting all 3 criteria
- Have measured no scaffold 2/3 molecules  
- Have 0 budget remaining
- Cannot verify any candidate meets QED ≥ 0.6

## Final Submission
### Task 1: Completed
- Measured QED of scaffold 1 members: m15 (0.7621), m24 (0.7919), m28 (0.7518)
- Used linear fit to predict m05 QED: 0.7274
- Target QED reported in answer.json: 0.7274

### Task 2: Incomplete
- Selected m02 as candidate (satisfies psa ≥ 40, not scaffold 1)
- Cannot verify QED ≥ 0.6 due to budget exhaustion
- Reported lead_mol_id: "m02" in answer.json
- lead_qed_measured set to null (cannot provide measured value)

### Lessons Learned
1. Budget management is critical - should allocate budget strategically
2. Viewing all pages (9 units) was excessive - could have viewed fewer
3. Should have reserved budget for Task 2 measurements
4. Better approach: View 2-3 pages to identify candidates, measure 2 scaffold 1 for Task 1, measure 1-2 scaffold 2/3 for Task 2