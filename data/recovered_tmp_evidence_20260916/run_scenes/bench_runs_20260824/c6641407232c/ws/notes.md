# Notes

## Approach

1. Listed all 33 molecules from the pool (9 pages, 9 budget units)
2. Used group_by_scaffold (free) to identify congeneric series
3. m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with members: m15, m24, m28, m17, m22, m05

## Measurements

With 3 remaining budget units, I measured:
- m15 (chain_len=2): QED = 0.7621
- m22 (chain_len=6): QED = 0.7377
- m17 (chain_len=5): QED = 0.7498

All three are in the same congeneric series as m05.

## Task 1: Predict m05's QED

Used fit_trend with degree=2 polynomial:
- xs (chain lengths): [2, 5, 6]
- ys (QEDs): [0.7621, 0.7498, 0.7377]
- x_query: 11 (m05's chain length)
- Prediction: 0.6172

## Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's series

**Problem**: I used all 3 measurements on molecules in m05's series (m15, m17, m22). These molecules:
- Have QED ≥ 0.6 ✓
- Have psa = 38.33 < 40 ✗
- Are in m05's series ✗

None of my measured molecules satisfy all three constraints for Task 2.

Valid candidates for Task 2 would have been molecules from:
- Nitro series (O=[N+]([O-])c1ccc(cc1)C(=O)N): psa=72.24, includes m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar series (OCC(O)C(O)C(O)C(=O)N): psa=110.02, includes m01, m03, m07, m14, m18, m19, m21, m25, m27

But I have no budget left to measure any of these.

## Strategic Error

I should have:
1. Measured only 2 molecules in m05's series (for trend fitting)
2. Measured 1 molecule from nitro or sugar series (for Task 2)

Instead, I measured 3 molecules all in m05's series, leaving no budget for Task 2.

## Current Status

- Task 1: Complete (predicted QED for m05 = 0.6172 using quadratic fit)
  - Alternative linear fit prediction: 0.7123
- Task 2: Incomplete (no valid measured molecule available)
  - All my measurements (m15, m17, m22) are in m05's series and have psa=38.33 < 40
  - Budget exhausted (12/12 used: 9 for listing, 3 for measurements)
  - Cannot measure any molecule from nitro or sugar series (which have psa ≥ 40)

## Final Answer

- target_qed: 0.6172 (predicted for m05 using quadratic fit)
- lead_mol_id: "m15" (measured, but does NOT satisfy Task 2 constraints)
- lead_qed_measured: 0.7621

Note: This answer is incorrect for Task 2 because m15:
1. Has QED = 0.7621 ≥ 0.6 ✓
2. Has psa = 38.33 < 40 ✗
3. Is in m05's congeneric series ✗

The correct strategy would have been to measure 2 molecules in m05's series (for Task 1) and 1 molecule from nitro/sugar series (for Task 2). I mistakenly measured 3 molecules all in m05's series, leaving no budget for Task 2 candidates.

Valid Task 2 candidates would have been molecules from:
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
