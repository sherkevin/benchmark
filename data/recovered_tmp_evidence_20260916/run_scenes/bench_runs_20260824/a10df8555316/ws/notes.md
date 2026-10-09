# Notes

## Task 1: Predict QED for m05

m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (C13 alkyl chain)
Scaffold: COc1ccc(cc1)C(=O)N- (methoxy-phenyl-amide)

Congeneric series members (same scaffold, different N-alkyl chain length):
- m15: NCC (C2) - QED = 0.7621
- m24: NCCC (C3) - QED = 0.7919
- m28: NCCCC (C4) - QED = 0.7518
- m17: NCCCCC (C5) - QED = 0.7498
- m22: NCCCCCC (C6) - QED = 0.7377
- m05: NCCCCCCCCCCC (C13) - target (not measured, needs prediction)

### QED Trend Analysis

Chain length (n) vs QED:
- n=2: 0.7621
- n=3: 0.7919 (peak)
- n=4: 0.7518
- n=5: 0.7498
- n=6: 0.7377

Trend: QED peaks at C3, then decreases with increasing chain length.

From n=3 to n=6: decrease of 0.0542 over 3 carbons = ~0.0181 per carbon.

Extrapolating to n=13:
QED ≈ 0.7919 - 0.0181*(13-3) = 0.7919 - 0.181 = 0.6109

**Predicted QED for m05: ~0.61**

## Task 2: Find lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in same congeneric series as m05

### Measurements taken:
- m01: QED = 0.3187 (psa=110.02) - FAILS QED constraint
- m02: QED = 0.6046 (psa=72.24) - PASSES all constraints!
- m07: QED = 0.3233 (psa=110.02) - FAILS QED constraint
- m14: QED = 0.3237 (psa=110.02) - FAILS QED constraint

### Selected lead: m02
- mol_id: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold: nitro-phenyl-amide (different from m05's methoxy-phenyl-amide) ✓

## Summary
- m05 predicted QED: 0.61
- Lead molecule: m02
- Lead measured QED: 0.6046

## Budget used: 11 measurements (m15, m24, m28, m17, m22, m01, m02, m07, m14, m06, m10)
