# Medicinal Chemistry Task Notes

## Overview
- Total compounds: 33
- Budget: 12 units (shared between list_pool and measure_qed)
- Task 1: Predict QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC), which cannot be synthesized
- Task 2: Find a molecule with QED ≥ 0.6, PSA ≥ 40, not in same series as m05

## Scaffold Analysis
Found 4 main scaffolds:
1. COc1ccc(cc1)C(=O)N - includes m05 (chain lengths: 2-11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N - PSA = 72.24 (chain lengths: 2-10) 
3. OCC(O)C(O)C(O)C(=O)N - PSA = 110.02 (chain lengths: 2-10)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N - PSA = 29.1 (chain lengths: 2-10)

## Measurements Made
- m15 (chain len 2): QED = 0.7621
- m24 (chain len 3): QED = 0.7919  
- m28 (chain len 4): QED = 0.7518
- m05: probe_unavailable (as expected)

## Task 1 - QED Prediction for m05
Used linear regression on points (2, 0.7621), (3, 0.7919), (4, 0.7518)
Predicted QED for m05 (chain len 11): 0.7274

## Task 2 - Lead Compound Selection
Needed: QED ≥ 0.6, PSA ≥ 40, not in same series as m05
Compounds meeting PSA and series criteria:
- O=[N+]([O-])c1ccc(cc1)C(=O)N series: all have PSA = 72.24
- OCC(O)C(O)C(O)C(=O)N series: all have PSA = 110.02

Unfortunately, budget was exhausted after measuring 3 compounds for Task 1 prediction,
so no QED measurements were possible for Task 2 candidates.

Best candidate from available information: m09 (O=[N+]([O-])c1ccc(cc1)C(=O)NCC)
- Not in same series as m05 ✓
- PSA = 72.24 ≥ 40 ✓
- Cannot confirm QED ≥ 0.6 (no measurement possible due to budget)

Alternative candidate: m14 (OCC(O)C(O)C(O)C(=O)NCC)
- Not in same series as m05 ✓  
- PSA = 110.02 ≥ 40 ✓
- Cannot confirm QED ≥ 0.6 (no measurement possible due to budget)