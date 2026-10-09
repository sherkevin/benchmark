# Notes for Medicinal Chemistry Task

## Initial Analysis
- Total budget: 17 units (shared between list_pool and measure_qed)
- 33 molecules in pool, 4 groups by scaffold
- m05 is in group 1 (scaffold: COc1ccc(cc1)C(=O)N) with chain length 11
- Need to predict QED of m05 by fitting trend from other molecules in same series
- Need to find molecule with: QED ≥ 0.6, psa ≥ 40, not in same series as m05

## Plan
1. List pool to see descriptors (especially psa values)
2. Measure QED of some molecules in m05's series to establish trend
3. Use fit_trend to predict m05's QED
4. Find candidate molecules with psa ≥ 40
5. Measure QED of promising candidates to find one with QED ≥ 0.6
6. Ensure candidate is not in m05's series

## Current Status
- Budget used: 9/17 (all pages viewed)
- Budget remaining: 8 units
- All 33 molecules cataloged with descriptors

## Observations from Pool Listing
Molecules with psa ≥ 40 (potential candidates for Task 2):
1. Group 2 (scaffold O=[N+]([O-])c1ccc(cc1)C(=O)N): m02, m06, m09, m10, m12, m29, m31, m32, m33 (all psa = 72.24)
2. Group 3 (scaffold OCC(O)C(O)C(O)C(=O)N): m01, m03, m07, m14, m18, m19, m21, m25, m27 (all psa = 110.02)

Group 1 (m05's series) molecules: m15, m17, m22, m24, m28 (all psa = 38.33, so not eligible anyway)

Group 4 molecules: all have psa = 29.1 (not eligible)

So candidates must come from Group 2 or Group 3.

## Next Steps
1. Measure QED of some molecules in Group 1 (m05's series) to establish trend
2. Predict m05 QED using fit_trend
3. Measure QED of promising candidates from Groups 2 and 3

## Task 1 Progress
Measured QED of molecules in m05's series (Group 1):
- m15 (chain_len=2): 0.7621
- m24 (chain_len=3): 0.7919
- m28 (chain_len=4): 0.7518
- m17 (chain_len=5): 0.7498
- m22 (chain_len=6): 0.7377

Predictions for m05 (chain_len=11):
- Linear fit (5 points): 0.6950
- Quadratic fit (5 points): 0.5416
- Linear fit (4 points): 0.7061
- Quadratic fit (4 points): 0.2689

The linear fit with 5 points (0.6950) is chosen as it uses all available data and has reasonable RMSE (0.01308). While quadratic fit suggests lower QED due to curvature, the linear trend is more conservative and better supported by the data.

## Task 2 Progress
Measured QED of candidates:
- m09 (Group 2, chain_len=2): QED = 0.5826 (< 0.6, not suitable)
- m02 (Group 2, chain_len=3): QED = 0.6046 (≥ 0.6, suitable!)

m02 satisfies all criteria:
1. QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24)
3. Not in same series as m05 ✓ (Group 2 vs Group 1)

## Final Budget Status
- Total budget: 17 units
- Used: 16 units (9 pages + 7 measurements)
- Remaining: 1 unit

## Decisions
- target_qed: 0.6950 (linear prediction for m05)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046

## Summary
Task completed successfully:
1. Predicted QED of m05 as 0.6950 using linear regression on QED measurements of 5 molecules in the same congeneric series (chain lengths 2, 3, 4, 5, 6).
2. Found m02 as a suitable lead molecule with QED=0.6046 (≥0.6), psa=72.24 (≥40), and not in the same series as m05 (Group 2 vs Group 1).

All deliverables completed:
- notes.md: This file documenting the process
- answer.json: Updated with correct values