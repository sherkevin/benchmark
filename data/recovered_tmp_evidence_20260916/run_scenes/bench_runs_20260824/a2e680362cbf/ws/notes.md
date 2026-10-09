# Notes for Medicinal Chemistry Task

## Task Overview

We need to:
1. Report the QED of m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - it has never been synthesized
2. Pick one molecule from the pool that satisfies all three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in the same congeneric series as m05 (different scaffold or not just N-alkyl chain length difference)

We have a budget of 12 measurements total for both tasks.

## Initial Observations

Looking at data/pool.tsv:
- Total 33 molecules
- m05 has: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1
- Need to find m05's QED despite it not being synthesized
- Need to find another molecule with psa ≥ 40 (m05 has psa=38.33 which is < 40)

## Strategy

### For Task 1:
m05 is in series with scaffold "COc1ccc(cc1)C(=O)N". Other members:
- m15 (chain_len=2)
- m24 (chain_len=3)
- m28 (chain_len=4)
- m17 (chain_len=5)
- m22 (chain_len=6)

We need to measure QED for some of these to predict m05's QED (chain_len=11). We should measure strategically to get a good trend.

### For Task 2:
Need to find a molecule that:
1. Has psa ≥ 40 (filter from pool)
2. Is not in same series as m05 (different scaffold)
3. Has QED ≥ 0.6 (need to measure)

From looking at data, molecules with psa ≥ 40 seem to be in series 2 and 3:
- Series 2: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (all have psa=72.24)
- Seriesарный 3: "OCC(O)C(O)C(O)C(=O)N" (all have psa=110.02)

Will need to efficiently use our 12 measurements total for both tasks.

## Measurements Taken

### For Task 1 (m05 series):
1. m15 (chain_len=2): QED=0.7621
2. m24 (chain_len=3): QED=0.7919
3. m28 (chain_len=4): QED=0.7518
4. m17 (chain_len=5): QED=0.7498
5. m22 (chain_len=6): QED=0.7377

Predictions for m05 (chain_len=11):
- With all 5 points, degree=2: 0.5416
- With all 5 points, degree=1 (linear): 0.695
- With first 4 points (2,4,5,6), degree=2: 0.6698

### For Task 2 (search for candidate):
6. m09 (Series 2, chain_len=2): QED=0.5826 (< 0.6)
7. m02 (Series 2, chain_len=3): QED=0.6046 (≥ 0.6) ✓
8. m14 (Series 3, chain_len=2): QED=0.3237 (< 0.6)
9. m12 (Series 2, chain_len=4): QED=0.4709 (< 0.6)
10. m32 (Series 2, chain_len=5): QED=0.4683 (< 0.6)
11. m33 (Series 2, chain_len=6): QED=0.4591 (< 0.6)
12. m29 (Series 2, chain_len=7): QED=0.4446 (< 0.6)

## Final Decisions

### Task 1: Predicted QED for m05
Using points from m05's series (excluding outlier m24): m15(2), m28(4), m17(5), m22(6)
- Quadratic fit (degree=2) gives prediction: 0.6698
- This has lowest RMSE (0.00174) and seems most reliable
- m24 (chain_len=3, QED=0.7919) appears to be an outlier in the trend

### Task 2: Selected molecule
m02 satisfies all constraints:
1. QED=0.6046 ≥ 0.6 ✓ (measured)
2. psa=72.24 ≥ 40 ✓ (from data)
3. Different scaffold from m05 (Series 2 vs Series 1) ✓

No other measured molecule in Series 2 or 3 had QED ≥ 0.6.

## Answer Summary
- target_qed (m05): 0.6698
- lead_mol_id: m02
- lead_qed_measured: 0.6046