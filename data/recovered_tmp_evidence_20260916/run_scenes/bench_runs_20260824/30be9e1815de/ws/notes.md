# Task Notes

Starting the medicinal chemistry task. Need to:

1. Determine the QED of m05 (which cannot be measured directly)
2. Find a molecule that meets all three criteria:
   - Measured QED ≥ 0.6
   - PSA ≥ 40
   - Not in the same congeneric series as m05

Budget: 17 units total for both listing pages and measurements.

From group_by_scaffold, I found that m05 belongs to the first group with scaffold "COc1ccc(cc1)C(=O)N" and has a chain length of 11.
The other members of this series are:
- m15 (chain_len: 2, qed: 0.7621)
- m24 (chain_len: 3, qed: 0.7919)
- m28 (chain_len: 4, qed: 0.7518)
- m17 (chain_len: 5, qed: 0.7498)
- m22 (chain_len: 6, qed: 0.7377)
- m05 (chain_len: 11, qed: ?)

Using these data points to predict m05's QED with fit_trend.
Predicted QED for m05: 0.5416

For Task 2, I identified m02 as meeting all three criteria:
1. Has measured QED = 0.6046 ≥ 0.6
2. Has PSA = 72.24 ≥ 40
3. Is in the second series (not the same as m05's series)

Another tested compound m01 had QED = 0.3187 which is below the threshold.