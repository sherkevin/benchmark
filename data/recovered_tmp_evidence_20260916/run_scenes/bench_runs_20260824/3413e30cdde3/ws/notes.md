# Notes on QED Prediction for m05 and Lead Selection

## Task Overview
1. Predict QED for m05 (which cannot be measured directly)
2. Pick a backup lead compound with QED ≥ 0.6, psa ≥ 40, not in m05's series

## Identifying m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

The scaffold is `COc1ccc(cc1)C(=O)N-` (methoxy-phenyl amide). The chain varies in length.

Molecules in the same series (same scaffold, different chain lengths):
- m15: `COc1ccc(cc1)C(=O)NCC` - chain length 2 (ethyl)
- m24: `COc1ccc(cc1)C(=O)NCCC` - chain length 3 (propyl)
- m28: `COc1ccc(cc1)C(=O)NCCCC` - chain length 4 (butyl)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - chain length 5 (pentyl)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - chain length 6 (hexyl)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - chain length 12 (target, not measurable)

Chain length = number of carbons in the N-alkyl chain.

## Plan
1. Measure QED for all congeners (m15, m24, m28, m17, m22) - 5 measurements
2. Fit a trend (polynomial) to extrapolate to chain length 12
3. For lead selection: first filter by psa ≥ 40 and not in m05 series, then measure QED

## Other Scaffolds Identified
- Nitro-phenyl amide: `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (m02, m06, m09, m10, m12, m29, m31, m32, m33)
- Biphenyl amide: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (m04, m08, m11, m13, m16, m20, m23, m26, m30)
- Sugar-derived amide: `OCC(O)C(O)C(O)C(=O)N-` (m01, m03, m07, m14, m18, m19, m21, m25, m27)

## Measurements Taken

### Congeners (m05 series) QED measurements:
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

### Trend Fitting
Fit quadratic polynomial (degree=2) through points (chain_length, QED):
- Points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
- Quadratic coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188
- Prediction at chain length 12: **0.4836**

Linear fit gave 0.6859 but quadratic fits the data better (lower RMSE) and the task explicitly says not to assume a straight line. The data shows a peak at chain 3 then decline, which the quadratic captures.

### Lead Selection
Screened 18 candidates with psa ≥ 40. None were in m05's series (m05 series molecules m15, m17, m22, m24, m28 don't have psa ≥ 40 or weren't in the passing list).

Measured QED for candidates:
- m02: QED = 0.6046 ✓ (≥ 0.6)
- m09: QED = 0.5826 ✗ (< 0.6)
- m12: QED = 0.4709 ✗ (< 0.6)

**Selected lead: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05 series (nitro-phenyl scaffold, not methoxy-phenyl) ✓

## Final Answers
- target_qed (m05 predicted): 0.4836
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
