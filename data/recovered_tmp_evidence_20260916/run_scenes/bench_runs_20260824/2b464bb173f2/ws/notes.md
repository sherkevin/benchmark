Medicinal Chemistry Task Notes

Task 1: Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- m05 has never been synthesized, so direct measurement returns probe_unavailable
- Used group_by_scaffold() to identify that m05 belongs to a series with scaffold "COc1ccc(cc1)C(=O)N"
- This series includes m15, m24, m28, m17, m22, and m05 with varying N-alkyl chain lengths
- Measured QED for several molecules in the series:
  - m15 (chain_len=2): QED = 0.7621
  - m24 (chain_len=3): QED = 0.7919
  - m28 (chain_len=4): QED = 0.7518
  - m17 (chain_len=5): QED = 0.7498
  - m22 (chain_len=6): QED = 0.7377
- Used fit_trend() to predict QED for m05 (chain_len=11)
- Predicted QED for m05: 0.5416

Task 2: Find a molecule meeting all three criteria:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same congeneric series as m05

- Identified that molecules in the same series as m05 have psa = 38.33 (less than 40)
- Other series with psa ≥ 40:
  - Nitrobenzene series (O=[N+]([O-])c1ccc(cc1)C(=O)N): psa = 72.24
  - Polyol series (OCC(O)C(O)C(O)C(=O)N): psa = 110.02
- Measured QED values for nitrobenzene series:
  - m09: QED = 0.5826 (less than 0.6)
  - m02: QED = 0.6046 (meets criterion!)
  - m12: QED = 0.4709 (less than 0.6)
  - m32: QED = 0.4683 (less than 0.6)
- Measured QED values for polyol series:
  - m14: QED = 0.3237 (less than 0.6)
  - m27: QED = 0.3330 (less than 0.6)
  - m01: QED = 0.3187 (less than 0.6)

Result: m02 is the best candidate satisfying all three criteria:
- mol_id: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED: 0.6046 ≥ 0.6 ✓
- psa: 72.24 ≥ 40 ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold) ✓