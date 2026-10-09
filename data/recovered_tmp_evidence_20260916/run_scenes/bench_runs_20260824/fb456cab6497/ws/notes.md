# Notes for Molecular Design Task

## Initial Analysis
- Need to determine the QED of m05 (not directly measurable since it's not synthesized)
- Need to find a molecule that meets all three criteria:
  1. QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in same congeneric series as m05

## Strategy
1. First, I'll explore the molecular pool to understand the scaffolds and identify the series containing m05
2. Use group_by_scaffold() to see the structural relationships
3. For m05, I'll need to find its congeneric series and predict its QED based on trends
4. Then search for a suitable lead molecule meeting all criteria

## Grouping Results
- Series 1 (COc1ccc(cc1)C(=O)N): m15, m24, m28, m17, m22, m05
- Series 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Series 3 (OCC(O)C(O)C(O)C(=O)N): m14, m27, m01, m19, m03, m21, m18, m07, m25
- Series 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13, m11, m04, m30, m23, m20, m26, m08, m16

## Task 1: Predicting QED of m05
Molecules in the same series as m05:
- m15: chain_len = 2, qed = 0.7621
- m24: chain_len = 3, qed = 0.7919
- m28: chain_len = 4, qed = 0.7518
- m17: chain_len = 5, qed = 0.7498
- m22: chain_len = 6, qed = 0.7377
- m05: chain_len = 11 (needs prediction)

Using fit_trend with chain lengths [2, 3, 4, 5, 6] and QEDs [0.7621, 0.7919, 0.7518, 0.7498, 0.7377], predicting for chain length 11:
- Predicted QED for m05: 0.5416

## Task 2: Finding a suitable lead molecule
Criteria: QED ≥ 0.6, PSA ≥ 40, not in same series as m05

From Series 2 (NO2-phenyl-carboxamide):
- m02: QED = 0.6046, PSA = 72.24, not in same series as m05 ✓

m02 meets all three criteria:
1. QED ≥ 0.6 (0.6046 ≥ 0.6) ✓
2. PSA ≥ 40 (72.24 ≥ 40) ✓
3. Not in same series as m05 (Series 2 vs Series 1) ✓

## Final Answer
- target_qed (m05): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046