# Analysis Notes

## Job 1 - Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to a congeneric series with the methoxyphenyl-CONH-alkyl scaffold. The congeners in the pool are:
- m15: COc1ccc(cc1)C(=O)NCC (2-carbon chain) - QED = 0.7621
- m24: COc1ccc(cc1)C(=O)NCCC (3-carbon chain) - QED = 0.7919
- m28: COc1ccc(cc1)C(=O)NCCCC (4-carbon chain) - QED = 0.7518
- m17: COc1ccc(cc1)C(=O)NCCCCC (5-carbon chain) - QED = 0.7498
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6-carbon chain) - QED = 0.7377

Pattern observed: The QED initially increases from 2 to 3 carbon atoms (0.7621 → 0.7919), then gradually decreases as the chain gets longer (0.7919 → 0.7518 → 0.7498 → 0.7377).

The relationship appears to be non-linear: QED increases initially as the chain lengthens from 2 to 3 carbon atoms, likely due to improved lipophilicity balancing, but then decreases as the chain becomes longer, possibly due to increased hydrophobicity and reduced solubility.

Since m05 has an 11-carbon chain (much longer than the measured 2-6 carbon chains), and the trend shows decreasing QED with increasing chain length after the initial peak at 3-carbons, I extrapolate that m05 will have a lower QED than m22 (0.7377). Based on the rate of decrease from 4-6 carbon chains (from 0.7518 to 0.7377), I estimate that extending to 11 carbons will result in a further decrease. My prediction for m05's QED is approximately 0.65.

## Job 2 - Picking a Lead Compound

I needed to find a molecule that satisfies all three criteria:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same series as m05

I systematically checked molecules with psa ≥ 40 that are not in m05's series and measured their QED:

From the nitrophenyl series (O=[N+]([O-])c1ccc(cc1)C(=O)N...):
- m09: psa = 72.24, qed = 0.5826 (FAILS - QED < 0.6)
- m10: psa = 72.24, qed = 0.3724 (FAILS - QED < 0.6)
- m12: psa = 72.24 (not measured, out of budget)
- m29: psa = 72.24, qed = 0.4446 (FAILS - QED < 0.6)
- m31: psa = 72.24, qed = 0.4252 (FAILS - QED < 0.6)
- m32: psa = 72.24 (not measured, out of budget)
- m33: psa = 72.24 (not measured, out of budget)

From the sugar alcohol series (OCC(O)C(O)C(O)C(=O)N...):
- m01: psa = 110.02, qed = 0.3187 (FAILS - QED < 0.6)
- m03: psa = 110.02, qed = 0.3282 (FAILS - QED < 0.6)
- m07: psa = 110.02 (not measured, out of budget)
- m14: psa = 110.02, qed = 0.3237 (FAILS - QED < 0.6)
- m18: psa = 110.02 (not measured, out of budget)
- m19: psa = 110.02 (not measured, out of budget)
- m21: psa = 110.02 (not measured, out of budget)
- m25: psa = 110.02 (not measured, out of budget)
- m27: psa = 110.02 (not measured, out of budget)

Unfortunately, none of the molecules I was able to measure satisfied all three criteria. The closest was m09 with QED = 0.5826. However, since I've exhausted my measurement budget and none of the measured compounds met the QED ≥ 0.6 threshold, I need to select the best option available.

Since I couldn't find a suitable lead compound that meets all three criteria, I'll need to select the best available candidate. However, I must strictly adhere to the constraint that all three must be met. Since none of the measured compounds met all criteria, I'll need to make an assumption based on structural similarity.

Actually, re-examining the results, no compound I tested met all three requirements. Since the task requires finding a compound that meets all three, I should note that m09 comes closest but doesn't quite reach the QED threshold.