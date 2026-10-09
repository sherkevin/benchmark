# Notes

## Task Overview
- Task 1: Predict QED for m05 (not synthesized, must use congeneric series trend)
- Task 2: Find one molecule with measured QED ≥ 0.6, psa ≥ 40, NOT in m05's congeneric series
- Budget: 12 units total (shared between list_pool and measure_qed)

## Pool Analysis
Read all 33 molecules (9 pages × 1 unit = 9 budget units used).

### m05 Congeneric Series (scaffold: COc1ccc(cc1)C(=O)N-)
Molecules in same series as m05:
- m15: NCC (2C), psa=38.33
- m24: NCCC (3C), psa=38.33
- m28: NCCCC (4C), psa=38.33
- m17: NCCCCC (5C), psa=38.33
- m22: NCCCCCC (6C), psa=38.33
- m05: NCCCCCCCCCCC (13C), psa=38.33

### Candidates for Task 2 (psa ≥ 40, not in m05 series)
Molecules with psa ≥ 40:
- m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

All of these have different scaffolds than m05, so they qualify if they have measured QED ≥ 0.6.

## Measurements Taken
Used remaining 3 budget units on:
- m15: QED = 0.7621 (in m05 series, psa=38.33 < 40)
- m28: QED = 0.7518 (in m05 series, psa=38.33 < 40)
- m17: QED = 0.7498 (in m05 series, psa=38.33 < 40)

## Strategic Error
I mistakenly measured molecules from m05's congeneric series to predict m05's QED, but this left me with no measurements for Task 2 candidates. All three measured molecules are in m05's series AND have psa < 40, so none qualify for Task 2.

## Task 1: m05 QED Prediction
Based on the trend from the congeneric series:
- m15 (2C): 0.7621
- m28 (4C): 0.7518
- m17 (5C): 0.7498

The QED decreases as chain length increases. Extrapolating to m05 (13C):
- Rate of decrease slows as chain gets longer
- Estimated QED for m05: ~0.70-0.73

Prediction: **0.72** (approximately)

## Task 2: Issue
No valid measurements for qualifying molecules (psa ≥ 40, not in m05 series, QED ≥ 0.6).

Budget exhausted (12/12 used). Cannot measure any more molecules.

This is a strategic error - should have measured some candidates with psa ≥ 40 first to find a valid lead molecule.

## Conclusion
- Task 1: Predicted m05 QED ≈ 0.71 (based on congeneric series trend: m15=0.7621, m28=0.7518, m17=0.7498)
- Task 2: Cannot complete correctly - budget exhausted before measuring any molecule with psa ≥ 40 not in m05's series

## Final Answer
- target_qed: 0.71 (predicted for m05)
- lead_mol_id: null (no valid measurement)
- lead_qed_measured: null (no valid measurement)

Note: Strategic error - should have measured candidates with psa ≥ 40 first before exhausting budget on m05's series.
