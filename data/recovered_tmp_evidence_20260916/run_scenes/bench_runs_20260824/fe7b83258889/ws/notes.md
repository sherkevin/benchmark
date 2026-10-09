# Notes

## Task Overview
- Task 1: Predict QED of m05 (cannot be measured directly, must use congeneric series trend)
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's series
- Budget: 12 units total (shared between list_pool and measure_qed)

## Approach Taken

### Step 1: Read the compound pool
- Used list_pool for pages 1-9 to get all 33 molecules
- Budget used: 9 units

### Step 2: Group by scaffold (free)
- Identified 4 scaffold groups:
  1. COc1ccc(cc1)C(=O)N: m15, m24, m28, m17, m22, m05 (m05's series)
  2. O=[N+]([O-])c1ccc(cc1)C(=O)N: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
  3. OCC(O)C(O)C(O)C(=O)N: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)
  4. c1ccc(cc1)-c1ccc(cc1)C(=O)N: m04, m08, m11, m13, m16, m20, m23, m26, m30 (psa=29.1)

### Step 3: Measure molecules from m05's series for Task 1
- Measured m15 (chain_len=2): QED = 0.7621
- Measured m24 (chain_len=3): QED = 0.7919
- Measured m28 (chain_len=4): QED = 0.7518
- Budget used: 3 units
- Total budget: 9 + 3 = 12 (exhausted)

### Step 4: Fit trend for m05 prediction
- Used linear fit (degree=1) with points: (2, 0.7621), (3, 0.7919), (4, 0.7518)
- Predicted QED for m05 (chain_len=11): 0.7274

### Step 5: Task 2 - PROBLEM
- Budget exhausted after measuring m15, m24, m28
- Could not measure any molecule outside m05's series
- All measured molecules (m15, m24, m28) are in m05's series and have psa=38.33 < 40
- None of the measured molecules satisfy Task 2 constraints

## Strategic Error
I should have measured only 2 molecules from m05's series (for linear fit) and saved 1 budget unit for measuring a Task 2 candidate. The optimal strategy would be:
- 9 units for list_pool
- 2 units for measuring m05 series members (e.g., m15, m24)
- 1 unit for measuring a Task 2 candidate (e.g., m02 with psa=72.24)

Total: 12 units

## Task 2 Candidates (psa ≥ 40, not in m05's series)
- From scaffold group 2 (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- From scaffold group 3 (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

Without measurements, I cannot verify which of these have QED ≥ 0.6.

## Conclusion
- Task 1: Completed with predicted QED = 0.7274 for m05 (using linear fit on m15, m24, m28)
- Task 2: Cannot complete due to budget exhaustion; no qualifying molecule was measured
- Final answer.json: target_qed=0.7274, lead_mol_id="m02" (candidate), lead_qed_measured=null (not measured)
- Strategic error: Should have measured only 2 molecules from m05's series (e.g., m15, m24) and saved 1 budget unit for measuring a Task 2 candidate (e.g., m02 with psa=72.24)
- Optimal budget allocation: 9 (list_pool) + 2 (Task 1 measurements) + 1 (Task 2 measurement) = 12 units
- Actual budget allocation: 9 (list_pool) + 3 (Task 1 measurements) + 0 (Task 2 measurements) = 12 units
