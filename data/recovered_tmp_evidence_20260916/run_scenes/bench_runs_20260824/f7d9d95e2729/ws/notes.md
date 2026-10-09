# Task Analysis Notes

## Identifying Congeneric Series for m05

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
This has a methoxybenzene-amide scaffold with a 12-carbon alkyl chain.

Based on SMILES analysis, molecules sharing the same scaffold (COc1ccc(cc1)C(=O)N) are:
- m15: COc1ccc(cc1)C(=O)NCC (2C chain)
- m24: COc1ccc(cc1)C(=O)NCCC (3C chain)
- m28: COc1ccc(cc1)C(=O)NCCCC (4C chain)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5C chain)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6C chain)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (12C chain) - target

Chain lengths (carbon atoms after N):
- m15: 2C
- m24:裁C
- m28: 4C
- m17: 5C
- m22: 6C
- m05: 12C (target)

All other molecules have different scaffolds:
- OCC(O)C(O)C(O)C(=O)N pattern (glycerol-like)
- O=[N+]([O-])c1ccc(cc1)C(=O)N pattern (nitrobenzene)
- c1ccc(cc1)-c1ccc(cc1)C(=O)N pattern (biphenyl)

## Measurement Results for Congeneric Series

Chain length (C atoms after N) and measured QED:
- m15: 2C chain, QED = 0.7621
- m24: 3C chain, QED = 0.7919
- m28: 4C chain, QED = 0.7518
- m17: 5C chain, QED = 0.7498
- m22: 6C chain, QED = 0.7377

Observations: QED peaks at 3-carbon chain (0.7919) then decreases with longer chains.

Target m05 has 12C chain - need to extrapolate beyond measured range (2-6C to 12C).

## QED Trend Analysis and Prediction for m05

Data points:
Chain lengths (xs): [2, 3, 4, 5, 6]
QED values (ys): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Quadratic fit (degree=2): predicts QED = 0.4836 for 12C chain (m05)
Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
RMSE: 0.01188

Linear fit (degree=1): predicts QED = 0.6859 for 12C chain
Coefficients: 0.79502 - 0.00909*x
RMSE: 0.01308

Decision: The quadratic fit has slightly lower RMSE and better captures the peak at 3C and subsequent decline. For a very long alkyl chain (12C), the quadratic prediction of 0.4836 seems more plausible than linear (0.6859), as extremely long chains typically reduce drug-likeness.

Will use quadratic prediction: target_qed = 0.4836 for m05.

## Measurement Plan for Job 2

## Job 2: Lead Compound Selection

### Screening Results
Using screen_candidates() tool: 18 molecules pass psa ≥ 40 filter.

None of m05's congeneric series members have psa ≥ 40 (all have psa = 38.33), so all 18 candidates automatically satisfy constraint #3 (not in same series as m05).

### QED Measurements for Candidate Evaluation

Remaining budget after Job 1: 7 measurements.

Measured candidates:
1. m02 (nitrobenzene, 3C chain): QED = 0.6046 ✓ (meets QED ≥ 0.6)
2. m09 (nitrobenzene, 2C chain): QED = 0.5826 ✗ (below 0.6)
3. m14 (glycerol-like, 2C chain): QED = 0.3237 ✗ (below 0.6)
4. m32 (nitrobenzene, 5C chain): QED = 0.4683 ✗ (below 0.6)
5. m12 (nitrobenzene, 4C chain): QED = 0.4709 ✗ (below 0.6)
6. m27 (glycerol-like, 3C chain): QED = 0.333 ✗ (below 0.6)
7. m29 (nitrobenzene, 7C chain): QED = 0.4446 ✗ (below 0.6)

### Lead Compound Decision
m02 satisfies all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓ (from data)
3. Not in same series as m05 ✓ (nitrobenzene scaffold vs methoxybenzene)

m02 is selected as the lead compound.

## Final Answer Summary

Written to answer.json:
- target_qed: 0.4836 (predicted QED for m05 using quadratic fit)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"] (all pool molecules sharing m05's scaffold)

All 12 measurement budget units used:
- 5 for m05's congeneric series (Job 1)
- 7 for candidate evaluation (Job 2)