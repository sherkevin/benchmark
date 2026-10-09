# Notes for QED Prediction Task

## Step 1: Identify Congeneric Series Members

From examining the pool and using `describe_structure`, I've identified molecules sharing m05's scaffold (`COc1ccc(cc1)C(=O)N`):

1. m15: chain length 2
2. m24: chain length 3  
3. m28: chain length 4
4. m17: chain length 5
5. m22: chain length 6
6. m05: target molecule, chain length 11 (not measurable)

These form the congeneric series for m05. The pool contains shorter chain lengths (2-6) while m05 has chain length 11, so we need to extrapolate.

## Step 2: Budget Planning

Total budget: 17 measurements
Already used: 9 budget units for list_pool (pages 1-9)
Remaining budget: 8 measurements

We need to:
1. Measure QED for congeneric series members to establish trend (Job 1)
2. Find a lead compound meeting all constraints (Job 2)

We should measure enough points in the congeneric series to establish a reliable trend, then use remaining budget to find lead.

## Step 3: Measuring Congeneric Series QED

Measured QED values for congeneric series members:

- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED =ог.7919  
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

Budget used: 14 total measurements (9 for list_pool + 5 for QED measurements)
Remaining budget: 3 measurements

The QED values show a pattern: after an initial increase from chain length 2 to 3, QED decreases as chain length increases from 3 to 6.

## Step 4: Fitting Trend and Predicting m05 QED

Fitted trends for QED vs chain length:

Using all 5 points (chain lengths 2-6):
- Degree 2 (quadratic): QED = 0.5416 at chain length 11, RMSE = 0.01188
- Degree 1 (linear): QED = 0.695 at chain length 11, RMSE = 0.01308

Using only chain lengths 3-6 (decreasing region):
- Degree 2: QED = 0.9378 at chain length 11, RMSE = 0.00539 (likely overfitted)

The quadratic fit with all points shows the best RMSE and accounts for the peak at chain length 3 followed by decline. The predicted QED of 0.5416 seems reasonable as longer alkyl chains typically reduce drug-likeness.

**Decision: Use quadratic fit with all points -> predicted QED for m05 = 0.5416**

## Step 5: Finding Lead Compound

Found candidate m02 meets all constraints:

1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓  
3. Not in same series as m05: scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro group) vs m05's `COc1ccc(cc1)C(=O)N` (methoxy group) ✓

Other measurements attempted:
- m01: QED = 0.3187 (too low)
- m02: QED = 0.6046 (acceptable)

Budget used: 16 total measurements, 1 remaining unused.

m02 is a valid lead compound meeting all three constraints.

## Summary

**Job 1 - Predicting m05 QED:**
- Identified congeneric series members: m15 (chain length 2), m24 (3), m28 (4), m17 (5), m22 (6)
- Measured QED values: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377 respectively
- Quadratic fit (degree 2) through all points gave best RMSE (0.01188)
- Predicted QED for m05 (chain length 11): **0.5416**

**Job 2 - Selecting Lead Compound:**
- Used screen_candidates() to find molecules with psa ≥ 40 (18 candidates)
- Eliminated molecules in m05's series (m15, m24, m28, m17, m22)
- Measured m01: QED = 0.3187 (too low)
- Measured m02: QED = 0.6046 (≥ 0.6, acceptable)
- Verified m02 scaffold different from m05: nitro vs methoxy
- **Selected lead: m02** with measured QED 0.6046

**Final answer.json submitted with:**
- target_qed: 0.5416
- lead_mol_id: "m02"  
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]