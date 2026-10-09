# Notes for QED Prediction Task

## Understanding the Data
- Data file contains 33 molecules with descriptors
- m05 is the target molecule with SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Budget: 12 total QED measurements for both tasks

## Scaffold Analysis
Used `group_by_scaffold()` and found 4 scaffold groups:
1. **COc1ccc(cc1)C(=O)N** (methoxybenzene scaffold) - includes m05
2. O=[N+]([O-])c1ccc(cc1)C(=O)N (nitrobenzene scaffold)
3. OCC(O)C(O)C(O)C(=O)N (polyol scaffold)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (biphenyl scaffold)

m05 is in group 1 with chain_len 11. Other members:
- m15 (chain_len 2)
- m24 (chain_len 3)
- m28 (chain_len 4)
- m17 (chain_len 5)
- m22 (chain_len 6)

## Task 1 Strategy
Need to predict QED for m05 by:
1. Measuring QED for molecules in same series
2. Fitting trend to chain length vs QED
3. Predicting at chain_len = 11

## Measurements for m05 Series
So far measured:
- m15 (chain_len 2): QED = 0.7621
- m24 (chain_len 3): QED = 0.7919
- m28 (chain_len 4): QED = 0.7518
- m17 (chain_len 5): QED = 0.7498
- m22 (chain_len 6): QED = 0.7377

Pattern: QED seems to peak around chain_len 3, then decreases gradually.

Predictions for m05 (chain_len 11):
- Linear (degree 1): 0.695
- Quadratic (degree 2): 0.5416
- Cubic (degree 3): 2.1323 (impossible, overfitting)

Linear prediction of 0.695 seems most reasonable given decreasing trend.

## Task 2 Analysis
Need molecule with:
1. QED ≥ 0.6 (measured)
2. psa ≥ 40
3. Not in same series as m05

PSA values by scaffold group:
- Group 1 (m05's series): psa = 38.33 < 40 ❌
- Group 2 (nitrobenzene): psa = 72.24 ≥ 40 ✅
- Group 3 (polyol): psa = 110.02 ≥ 40 ✅  
- Group 4 (biphenyl): psa = 29.1 < 40 ❌

So need to search in groups 2 or 3.

## Measurements for Task 2
Group 2 (nitrobenzene) measurements:
- m09 (chain_len 2): QED = 0.5826 < 0.6
- m02 (chain_len 3): QED = 0.6046 ≥ 0.6 ✅ FOUND CANDIDATE
- m12 (chain_len 4): QED = 0.4709 < 0.6
- m32 (chain_len 5): QED = 0.4683 < 0.6
- m33 (chain_len 6): QED = 0.4591 < 0.6

Group 3 (polyol) measurements:
- m14 (chain_len 2): QED = 0.3237 < 0.6
- m27 (chain_len 3): QED = 0.333 < 0.6

Best candidate found: m02 with QED = 0.6046

## Final Decisions
Task 1: m05 predicted QED = 0.695 (linear extrapolation from chain_len 2-6)
Task 2: m02 selected with measured QED = 0.6046

Budget used: All 12 measurements used.

## Summary
1. **Task 1**: Measured QED for 5 molecules in m05's series (m15, m24, m28, m17, m22) with chain lengths 2-6. Used linear regression to predict m05 (chain_len 11) QED = 0.695.
2. **Task 2**: Searched groups with psa ≥ 40 (groups 2 and 3). Found m02 in group 2 (nitrobenzene scaffold) with QED = 0.6046 ≥ 0.6, psa = 72.24 ≥ 40, and not in same series as m05.