# Notes

## Task 1: Find QED of m05

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl-amide (COc1ccc(cc1)C(=O)N-)
- N-alkyl chain: 13 carbons
- psa = 38.33

Congeneric series members measured:
- m15 (C2 chain): QED = 0.7621
- m24 (C3 chain): QED = 0.7919
- m28 (C4 chain): QED = 0.7518
- m17 (C5 chain): QED = 0.7498
- m22 (C6 chain): QED = 0.7377

Trend analysis:
- QED peaks at chain length 3 (0.7919)
- Then decreases with increasing chain length
- From C3 to C6: decrease of ~0.018 per carbon
- Extrapolating to C13: QED ≈ 0.7919 - 0.018×(13-3) = 0.7919 - 0.18 = 0.61

Estimated QED for m05: **0.61**

## Task 2: Find a lead molecule

Constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in same congeneric series as m05

Measurements made:
- m01: QED = 0.3187, psa = 110.02 → FAIL (QED < 0.6)
- m02: QED = 0.6046, psa = 72.24 → PASS all constraints!
- m03: QED = 0.3282, psa = 110.02 → FAIL (QED < 0.6)
- m07: QED = 0.3233, psa = 110.02 → FAIL (QED < 0.6)
- m09: QED = 0.5826, psa = 72.24 → FAIL (QED < 0.6)

**Selected: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold: nitro-phenyl-amide (different from m05's methoxy-phenyl) ✓

## Budget used
12 measurements total:
- 5 for m05 congeneric series (m15, m24, m28, m17, m22)
- 5 for Task 2 candidates (m01, m02, m03, m07, m09)
- 2 remaining (not needed)

## Final Answer
- target_qed: 0.61 (extrapolated from congeneric series trend)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
