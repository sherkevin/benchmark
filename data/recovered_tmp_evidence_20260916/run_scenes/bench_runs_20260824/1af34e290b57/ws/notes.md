# Medicinal Chemistry Task - Notes

## Task Overview
- Predict QED for m05 (never synthesized, chain_len=11 on scaffold 1)
- Find a lead molecule with QED ≥ 0.6, PSA ≥ 40, not in scaffold 1

## Scaffold Analysis
Grouped 33 molecules into 4 scaffolds:
1. **Scaffold 1** (m05's scaffold): `COc1ccc(cc1)C(=O)N` - 6 members (chain 2-6, 11)
2. **Scaffold 2**: `O=[N+]([O-])c1ccc(cc1)C(=O)N` - 9 members (chain 2-10)
3. **Scaffold 3**: `OCC(O)C(O)C(O)C(=O)N` - 9 members (chain 2-10)
4. **Scaffold 4**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` - 9 members (chain 2-10)

## Task 1: Predicting m05 QED

Measured all 5 synthesized members of scaffold 1:
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

Fitted trends to predict m05 at chain_len=11:
- **Quadratic fit**: QED = 0.5416 (RMSE = 0.01188)
- **Linear fit**: QED = 0.695 (RMSE = 0.01308)

Additional fits for sensitivity analysis:
- Chains 3-6, degree 2: QED = 0.9378 (unrealistic - QED > 0.9 for long chain)
- Chains 3-6, degree 1: QED = 0.6508
- Chains 4-6, degree 1: QED = 0.7041
- Chains 4-6, degree 2: QED = 0.5257 (perfect fit with 3 points)

**Decision**: Used quadratic prediction on all 5 points (0.5416) because:
1. Uses all available data (most robust)
2. Better RMSE than linear on full data
3. Captures the peak at chain 3 and subsequent decline
4. Physically reasonable - QED decreases with very long alkyl chains (alogp=4.956, rotb=12)
5. Consistent with quadratic fit on chains 4-6 (0.5257)

## Task 2: Finding a Lead Molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not scaffold 1

PSA analysis by scaffold:
- Scaffold 2: PSA = 72.24 (all members) ✓
- Scaffold 3: PSA = 110.02 (all members) ✓
- Scaffold 4: PSA = 29.1 (all members) ✗

Measured candidates from scaffold 2:
- **m02**: QED = 0.6046, PSA = 72.24, MW = 208.22, alogp = 1.735
  - Satisfies all three constraints ✓
- **m09**: QED = 0.5826, PSA = 72.24, MW = 194.19, alogp = 1.345
  - QED < 0.6, does not satisfy constraint 1 ✗

**Selected lead**: m02 (only measured candidate satisfying all constraints)

## Budget Usage
- 5 pages of list_pool (pages 1-5): 5 units
- 5 measurements of scaffold 1 members (m15, m24, m28, m17, m22): 5 units
- 2 measurements of lead candidates (m02, m09): 2 units
- **Total: 12/12 units used**

## Final Answers
- **m05 predicted QED**: 0.5416
- **Lead molecule**: m02 (measured QED = 0.6046)
