# Notes for Medicinal Chemistry Task

## Task Overview
1. Predict QED for m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - cannot be measured directly (probe_unavailable)
2. Find a molecule that satisfies:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in same congeneric series as m05 (same scaffold, only N-alkyl chain length differs)

## Budget: 12 measurements total for both tasks

## Initial Analysis
From data/pool.tsv:
- Total 33 molecules (m01-m33)
- m05 has: mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1
- m05 is in the series: COc1ccc(cc1)C(=O)N[alkyl chain]
- Molecules with same scaffold as m05: m15, m17, m22, m24, m28 (all COc1ccc(cc1)C(=O)N... patterns)
- Need to avoid these molecules for Task 2

## Task 1 Strategy
Need to predict QED for m05 based on trend within its congeneric series. Need to measure QED of other molecules in same series to establish trend.

Confirmed m05 returns probe_unavailable (no sample).

## Task 2 Strategy
Need to find molecule with psa ≥ 40, not in m05 series, and with measured QED ≥ 0.6.

## Measurements Strategy
Budget: 12 total measurements
Used: 5 (m15, m24, m28, m17, m22)
Available: 7 more measurements

## Task 1 Measurements (m05 series)
m15 (C2 chain): QED = 0.7621
m24 (C3 chain): QED = 0.7919  
m28 (C4 chain): QED = 0.7518
m17 (C5 chain): QED = 0.7498
m22 (C6 chain): QED = 0.7377

Chain lengths: C2, C3, C4, C5, C6
QED values: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377

m05 has C12 chain (longest). Need to predict QED based on trend.

## Trend Analysis for m05
C2: 0.7621
C3: 0.7919 (peak)
C4: 0.7518 (-0.0401)
C5: 0.7498 (-0.0020)
C6: 0.7377 (-0.0121)

Average decrease per carbon after C3: ~0.0181
From C6 to C12: 6 carbons × 0.0181 = 0.1086 decrease
Predicted QED for m05 (C12): 0.7377 - 0.1086 = 0.6291

But trend not perfectly linear. Alternative: extrapolate linear fit from C3-C6 data.

## Additional Measurements for Context
m10 (C10 in nitrobenzene series): QED = 0.3724
m16 (C10 in biphenyl series): QED = 0.4842

## Task 2 Candidate Confirmed!
m02 satisfies all conditions:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓  
3. not in same series as m05 (different scaffold: nitrobenzene vs methoxybenzene) ✓

## Final Prediction for m05 QED
Based on trend in m05 series:
- Average decrease per carbon after C3: ~0.0181
- From C6 to C12: 6 carbons × 0.0181 = 0.1086 decrease
- Prediction: 0.7377 - 0.1086 = 0.6291 ≈ 0.63

Considering high alogp (4.956) and rotb (12) of m05 vs C6 (alogp 3.005, rotb 7), QED likely lower.

Final prediction: m05 QED = 0.63

## Final Answers
- target_qed: 0.63 (predicted QED for m05)
- lead_mol_id: "m02" (molecule satisfying Task 2 conditions)
- lead_qed_measured: 0.6046 (measured QED for m02)

Measurements used: 10 total (m15, m24, m28, m17, m22, m01, m03, m02, m10, m16)
Budget remaining: 2 (unused)