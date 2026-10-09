# Drug Design Task Notes

## Initial Exploration
- Used `group_by_scaffold()` to identify 4 congeneric series
- Identified that m05 (target molecule) belongs to series 1 with scaffold `COc1ccc(cc1)C(=O)N`
- This series includes: m15(chain_len:2), m24(chain_len:3), m28(chain_len:4), m17(chain_len:5), m22(chain_len:6), m05(chain_len:11)

## Task 1: Predict QED of m05
- Confirmed m05 is not available for measurement (probe_unavailable)
- Measured QED of 3 compounds in the same series:
  - m15: QED = 0.7621
  - m24: QED = 0.7919  
  - m28: QED = 0.7518
- Used linear regression with these 3 points to predict m05's QED at chain length 11
- Prediction: 0.7274

## Task 2: Find molecule meeting all three criteria
- Criteria: QED ≥ 0.6, PSA ≥ 40, not in same series as m05
- Budget exhausted after Task 1 measurements, unable to measure candidates
- Series 3 molecules (scaffold `OCC(O)C(O)C(O)C(=O)N`) all have PSA = 110.02
- Series 2 molecules (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`) all have PSA = 72.24
- Series 4 molecules have PSA = 29.1 (too low)
- Could not measure any molecule from series 2 or 3 to meet Task 2 requirements
- Strategic error: should have allocated budget differently to measure candidates for Task 2

## Budget Usage
- List pool: 9 pages × 1 = 9 units
- Measure QED: 3 measurements × 1 = 3 units
- Total: 12/12 units used