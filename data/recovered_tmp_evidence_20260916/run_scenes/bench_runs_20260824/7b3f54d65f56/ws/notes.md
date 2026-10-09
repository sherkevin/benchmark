# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - chain length 12 carbons in alkyl group
- Find backup lead: QED ≥ 0.6, psa ≥ 40, NOT in m05 series
- Budget: 17 measurements total

## m05 Analysis
SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N- (methoxy-phenyl-amide)
- Alkyl chain: 12 carbons (CCCCCCCCCCCC)

## m05 Series Congeners Identified
Same scaffold COc1ccc(cc1)C(=O)N-:
- m15: NCC (2 carbons) - QED = 0.7621
- m24: NCCC (3 carbons) - QED = 0.7919
- m28: NCCCC (4 carbons) - QED = 0.7518
- m17: NCCCCC (5 carbons) - QED = 0.7498
- m22: NCCCCCC (6 carbons) - QED = 0.7377

## Trend Fitting for m05 QED Prediction
Fitted quadratic (degree=2) polynomial through points (chain_length, QED):
- Points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- Equation: QED(x) = 0.74932 + 0.017024*x - 0.003264*x²
- RMSE: 0.01188

**Prediction for m05 (chain length 12): QED = 0.4836**

This is an extrapolation well beyond the measured range (2-6 → 12). The trend shows QED peaks around chain length 3, then steadily decreases as chain gets longer. At chain length 12, the predicted QED drops to 0.4836.

## Backup Lead Selection

### PSA Constraint (psa ≥ 40)
- m05 series (COc1ccc...): psa = 38.33 - FAILS (also excluded by constraint 3)
- Nitro series O=[N+]([O-])c1ccc...: psa = 72.24 - PASSES
- Sugar series OCC(O)C(O)C(O)C(=O)N-: psa = 110.02 - PASSES
- Biphenyl series c1ccc(cc1)-c1ccc...: psa = 29.1 - FAILS

### Measurements for Lead Selection
- m02 (nitro, 3-carbon chain): QED = 0.6046 ✓ PASSES all constraints
- m01 (sugar, 4-carbon chain): QED = 0.3187 ✗ Below 0.6
- m09 (nitro, 2-carbon chain): QED = 0.5826 ✗ Below 0.6

### Selected Lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED measured: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Series: nitro-phenyl-amide (NOT m05 series ✓)

## Final Answer
- target_qed: 0.4836
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
