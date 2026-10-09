# Notes: Predicting QED for m05 and Selecting Lead Compound

## Step 1: Identify Congeneric Series for m05

Using `describe_structure`:
- m05: scaffold = "COc1ccc(cc1)C(=O)N", chain_len = 11
- Found pool members with same scaffold:
  - m15: chain_len = 2
  - m24: chain_len = 3  
  - m28: chain_len = 4
  - m17: chain_len = 5
  - m22: chain_len = 6

So the congeneric series for m05 includes: m15, m24, m28, m17, m22 (and m05 itself).

## Step 2: Plan Measurements for Trend Analysis

We need to measure QED for some members of this series to establish how QED changes with chain length. Since m05 has chain length 11 and pool members have chain lengths 2-6, we're extrapolating beyond measured range.

Budget: 12 total measurements for both jobs.

Initial measurement strategy:
1. Measure all 5 congenerics in the series to get good data for trend fitting
2. Use polynomial fit (degree 2 or higher as task suggests not assuming straight line)
3. Extrapolate to chain length 11 for m05

That's 5 measurements for job 1, leaving 7 for job 2.

## Step ▌Measurements for Congeneric Series

Measured QED values:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919  
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Observations: QED peaks around chain length 3 (0.7919) and then decreases as chain length increases to 6 (0.7377).

## Step 4: Trend Fitting and Extrapolation

Fitted polynomial trends:
- Degree 1 (linear): prediction = 0.6950, RMSE = 0.01308
- Degree 2 (quadratic): prediction = 0.5416, RMSE = 0.01188  
- Degree 3 (cubic): prediction = 2.1323 (invalid >1), RMSE = 0.00835

The quadratic fit is most reasonable:
- Captures the peak at chain length 3 and subsequent decrease
- Has lower RMSE than linear fit
- Gives prediction within valid QED range (0-1)
- Cubic overfits and gives unrealistic extrapolation (>1)

Predicted QED for m05 (chain_len=11): **0.5416**

## Step 5: Job 2 - Selecting Lead Compound

Constraints for lead compound:
1. measured QED ≥ 0.6 (must measure)
2. psa ≥ 40 (free from data)
3. not in same series as m05 (free, check scaffold)

### Screening Process

1. Used `screen_candidates()`: 18 molecules pass psa ≥ 40 constraint
2. These belong to two main scaffolds:
   - Hydroxy series: "OCC(O)C(O)C(O)C(=O)N" (m01, m03, m07, m14, m18, m19, m21, m25, m27)
   - Nitro series: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (m02, m06, m09, m10, m12, m29, m31, m32, m33)

### QED Measurements for Candidate Evaluation

Used 6 measurements for job 2 (11 total used, 1 remaining):

Hydroxy series:
- m01 (chain_len=4): QED = 0.3187 < 0.6
- m14 (chain_len=2): QED = 0.3237 < 0.6
- Hydroxy series appears to have low QED values

Nitro series:
- m09 (chain_len=2): QED = 0.5826 < 0.6
- m02 (chain_len=3): QED = 0.6046 ≥ 0.6 ✓
- m12 (chain_len=4): QED = 0.4709 < 0.6
- m29 (chain_len=7): QED = 0.4446 < 0.6

Nitro series shows peak QED around chain length 3.

### Selected Lead Compound: m02

m02 satisfies all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. scaffold = "O=[N+]([O-])c1ccc(cc1)C(=O)N" ≠ m05's scaffold ✓

No other measured candidate meets all criteria. Hydroxy series has low QED, and other nitro series members have QED < 0.6.

## Summary

### Job 1: Predict QED for m05
- Identified congeneric series: m15, m24, m28, m17, m22 (same scaffold as m05)
- Measured all 5 congenerics (chain lengths 2-6)
- QED trend: peaks at chain length 3 (0.7919), decreases with longer chains
- Quadratic fit (degree=2) predicts QED = 0.5416 for m05 (chain_len=11)
- Used 5 measurements

### Job 2: Select Lead Compound
- Screened for psa ≥ 40: 18 candidates
- Measured representatives from hydroxy and nitro series
- Found m02 meets all criteria:
  - QED = 0.6046 ≥ 0.6
  - psa = 72.24 ≥ 40  
  - Different scaffold from m05
- Used 6 measurements (total 11/12 used)

### Final Answer
- target_qed: 0.5416
- lead_mol_id: m02  
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]