# Notes

## Task 1: Predict QED for m05

m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so measure_qed returns probe_unavailable. I need to predict its QED from the congeneric series trend.

### Congeneric series members (same methoxy-phenyl scaffold, varying N-alkyl chain length):
| mol_id | chain length | QED (measured) |
|--------|--------------|----------------|
| m15    | 2 (NCC)      | 0.7621         |
| m24    | 3 (NCCC)     | 0.7919         |
| m28    | 4 (NCCCC)    | 0.7518         |
| m17    | 5 (NCCCCC)   | 0.7498         |
| m22    | 6 (NCCCCCC)  | 0.7377         |
| m05    | 13           | ? (predict)    |

### Trend analysis:
- QED peaks at 3 carbons (m24: 0.7919)
- QED decreases as chain length increases beyond 3 carbons
- m05 has a very long chain (13 carbons), high alogp (4.956), and high rotb (12)
- These properties significantly penalize drug-likeness

### Prediction for m05:
Based on the decreasing trend and the extreme chain length, I estimate m05's QED to be approximately **0.52**. The long hydrophobic chain and high lipophilicity would substantially reduce drug-likeness compared to shorter-chain analogs.

## Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in m05 series

### Measurements performed (budget: 12 total):
1. m15: QED = 0.7621 (congeneric series, for Task 1)
2. m24: QED = 0.7919 (congeneric series, for Task 1)
3. m28: QED = 0.7518 (congeneric series, for Task 1)
4. m17: QED = 0.7498 (congeneric series, for Task 1)
5. m22: QED = 0.7377 (congeneric series, for Task 1)
6. m01: QED = 0.3187 (psa=110.02, but QED < 0.6)
7. m02: QED = 0.6046 (psa=72.24, QED ≥ 0.6) ✓
8. m09: QED = 0.5826 (psa=72.24, but QED < 0.6)
9. m14: QED = 0.3237 (psa=110.02, but QED < 0.6)

### Selected molecule: m02
- **mol_id**: m02
- **SMILES**: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- **Measured QED**: 0.6046 (≥ 0.6) ✓
- **psa**: 72.24 (≥ 40) ✓
- **Not in m05 series**: m02 has nitro-phenyl scaffold, m05 has methoxy-phenyl scaffold ✓

All three constraints satisfied!

## Budget used: 9 out of 12 measurements
