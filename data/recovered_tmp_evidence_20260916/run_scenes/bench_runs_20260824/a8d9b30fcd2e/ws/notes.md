# Notes for QED Prediction and Lead Selection

## Initial Analysis

### Target Molecule m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: Methoxy-phenyl with amide linkage to alkyl chain
- Chain length: C12 (12-carbon alkyl chain)

### Congeneric Series Identification
Molecules sharing the methoxy-phenyl scaffold (`COc1ccc(cc1)C(=O)N`):
1. m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
2. m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)
3. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
4. m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
5. m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain) - target

Thus `congener_ids` should be: ["m15", "m17", "m22", "m24", "m28"]

### QED Measurements for m05's Congeneric Series
Measured QED values:
1. m15 (C2 chain): 0.7621
2. m24 (C3 chain): 0.7919 (peak)
3. m28 (C4 chain): 0.7518
4. m17 (C5 chain): 0.7498
5. m22 (C6 chain): 0.7377

Trend: QED peaks at C3 chain length (0.7919) then decreases with longer chains.

Chain length vs QED data:
- C2: 0.7621
- C3: 0.7919 (peak)
- C4: 0.7518
- C5: 0.7498
- C6: 0.7377

The decrease from C3 to C6 is 0.0542 over 3 carbons (avg -0.01807 per carbon).
From C4 to C6: -0.0141 over 2 carbons (avg -0.00705 per carbon).
From C5 to C6: -0.0121 over 1 carbon.

m05 has C12 chain, which is 6 carbons longer than C6. Extrapolating:
- Using C5→C6 rate (-0.0121 per carbon): predicted decrease = 6 × 0.0121 = 0.0726, predicted QED = 0.7377 - 0.0726 = 0.6651
- Using C4→C6 average rate (-0.00705 per carbon): predicted decrease = 6 × 0.00705 = 0.0423, predicted QED = 0.7377 - 0.0423 = 0.6954
- Using C3→C6 average rate (-0.01807 per carbon): predicted decrease = 6 × 0.01807 = 0.1084, predicted QED = 0.7377 - 0.1084 = 0.6293

Given the decreasing trend and the high alogp of m05 (4.956 vs 3.005 for C6), a QED around 0.65-0.68 seems reasonable.

**Final Prediction**: m05 QED = 0.665 based on:
- Linear extrapolation from C5→C6 trend (-0.0121 per carbon)
- 6 carbons beyond C6: 6 × 0.0121 = 0.0726 decrease
- Predicted QED = 0.7377 (C6) - 0.0726 = 0.6651 ≈ 0.665
- This aligns with observed decreasing trend and high alogp (4.956)

**Lead compound choice**: m02
- QED measured: 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓ (from pool.tsv)
- Not in m05's series (nitro-phenyl vs methoxy-phenyl) ✓

**Measurements used**: 9 total (5 congenerics + 4 lead candidates)
**Remaining budget**: 3 measurements

### Lead Compound Search
Candidates with psa ≥ 40:
- Nitro-phenyl series (psa = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Polyol series (psa = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

Measured m02 (nitro-phenyl, C3 chain): QED = 0.6046 ≥ 0.6
m02 satisfies all three constraints:
1. measured QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24 from pool.tsv)
3. not in m05's series ✓ (nitro-phenyl scaffold, not methoxy-phenyl)