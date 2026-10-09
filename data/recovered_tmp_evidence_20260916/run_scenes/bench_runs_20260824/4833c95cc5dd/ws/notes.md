# Notes for Medicinal Chemistry Task

## Task Overview
1. Predict QED for m05 (unsynthesized) using trends from its congeneric series
2. Find one molecule with: QED ≥ 0.6, psa ≥ 40, and different scaffold from m05

## Data Analysis
- Total molecules in pool: 33
- m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (methoxy-phenyl carbonyl with long N-alkyl chain)
- m05 descriptors: mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1

## Congeneric Series Analysis
Based on SMILES patterns, I identify several series:

1. **m05 series** (methoxy-phenyl carbonyl with varying N-alkyl chain):
   - m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (C12 chain)
   - m15: COc1ccc(cc1)C(=O)NCC (C2 chain)
   - m17: COc1ccc(cc1)C(=O)NCCCCC (C5 chain)
   - m22: COc1ccc(cc1)C(=O)NCCCCCC (C6 chain)
   - m24: COc1ccc(cc1)C(=O)NCCC (C3 chain)
   - m28: COc1ccc(cc1)C(=O)NCCCC (C4 chain)

2. **Other series**:
   - Nitro-phenyl carbonyl series (O=[N+]([O-])c1ccc(cc1)C(=O)N...): m02, m06, m09, m10, m12, m29, m31, m32, m33
   - Biphenyl carbonyl series (c1ccc(cc1)-c1ccc(cc1)C(=O)N...): m04, m08, m11, m13, m16, m20, m23, m26, m30
   - Polyol series (OCC(O)C(O)C(O)C(=O)N...): m01, m03, m07, m14, m18, m19, m21, m25, m27

## Task Strategy
1. First identify molecules in m05's congeneric series to measure their QED and establish trend
2. Use trend to predict m05's QED
3. For Task 2, identify molecules with psa ≥ 40 and different scaffold from m05
4. Measure QED for promising candidates to find one with QED ≥ 0.6

## Budget Constraint
- Total budget: 12 measurements
- Need to allocate wisely between Task 1 (establishing trend) and Task 2 (finding candidate)

## Initial Observations
- psa ≥ 40 requirement eliminates biphenyl series (all have psa=29.1)
- m05 series has psa=38.33, so also eliminated from Task 2
- Candidates must come from nitro-phenyl series (psa=72.24) or polyol series (psa=110.02)

## m05 Series QED Measurements
I've measured 5 molecules from m05's congeneric series:
- m15 (C2 chain): QED = 0.7621
- m17 (C5 chain): QED = 0.7498
- m22 (C6 chain): QED = 0.7377
- m24 (C3 chain): QED = 0.7919
- m28 (C4 chain): QED = 0.7518

Chain lengths (carbon atoms in alkyl chain):
- m15: 2 carbons
- m24: 3 carbons
- m28: 4 carbons
- m17:160 5 carbons
- m22: 6 carbons
- m05: 12 carbons (target)

Pattern: QED generally decreases with longer chain length, but m24 (C3) is highest.

## Budget Status
- Measurements used: 12
- Budget remaining: 0
- All measurements consumed

## Task 2 Candidate Screening
I measured several nitro-phenyl and polyol molecules:

**Nitro-phenyl series (psa=72.24):**
- m02: QED = 0.6046 ✓ (meets QED ≥ 0.6)
- m09: QED = 0.5826 ✗
- m12: QED = 0.4709 ✗
- m32: QED = 0.4683 ✗
- m06: QED = 0.4011 ✗
- m29: QED = 0.4446 ✗

**Polyol series (psa=110.02):**
- m01: QED = 0.3187 ✗

**Found candidate:** m02 meets all Task 2 criteria:
1. QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24)
3. Different scaffold from m05 ✓ (nitro-phenyl vs methoxy-phenyl)

## m05 QED Prediction
Based on m05 series trend:
- Chain length vs QED shows decreasing pattern
- m05 has longest chain (C12) with highest rotb (12) and alogp (4.956)
- Extrapolating from measured values: predicted QED ~0.68 (range 0.63-0.70)

Final prediction: target_qed = 0.68

## Final Answer
1. **Task 1**: Predicted QED for m05 = 0.68
   - Based on decreasing trend in its congeneric series (methoxy-phenyl carbonyls)
   - m05 has longest chain (C12) with highest rotb and alogp, suggesting lowest QED in series
   - Extrapolated from measured values: m15(0.7621), m24(0.7919), m28(0.7518), m17(0.7498), m22(0.7377)

2. **Task 2**: Selected m02 as lead molecule
   - QED measured: 0.6046 (≥ 0.6 requirement)
   - psa = 72.24 (≥ 40 requirement)
   - Different scaffold from m05: nitro-phenyl carbonyl vs methoxy-phenyl carbonyl
   - All constraints satisfied

Answer.json has been updated with these values.