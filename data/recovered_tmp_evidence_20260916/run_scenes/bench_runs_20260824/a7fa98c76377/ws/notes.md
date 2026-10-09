Medicinal chemistry task notes

Task overview:
- Need to find the QED for m05 (which cannot be measured directly)
- Need to find a molecule that meets three criteria:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. not in the same congeneric series as m05

Starting by exploring the compound pool to understand the scaffolds and relationships.

From group_by_scaffold, I found that m05 belongs to the first group with scaffold "COc1ccc(cc1)C(=O)N" and has a chain length of 11.

Molecules in the same series as m05 with their chain lengths and measured QED:
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377
- m05 (chain_len: 11): QED = 0.5416 (predicted)

For Task 1: m05 QED = 0.5416

Now moving to Task 2: Find a molecule that:
1. Has measured QED ≥ 0.6
2. Has psa ≥ 40
3. Is not in the same congeneric series as m05 (not in the "COc1ccc(cc1)C(=O)N" scaffold)

Explored various molecules:
- m09: QED = 0.5826 (< 0.6) ❌
- m02: QED = 0.6046 ≥ 0.6 ✓, psa = 72.24 ≥ 40 ✓, different series ✓
- m14: QED = 0.3237 (< 0.6) ❌
- m13: QED = 0.854 ≥ 0.6 ✓, psa = 29.1 < 40 ❌, different series ✓
- m27: QED = 0.333 (< 0.6) ❌
- m19: QED = 0.3244 (< 0.6) ❌
- m11: QED = 0.869 ≥ 0.6 ✓, psa = 29.1 < 40 ❌, different series ✓

Best candidate for Task 2: m02
- Meets all three criteria
- QED = 0.6046
- psa = 72.24
- Different congeneric series than m05

Summary:
- Task 1: Predicted QED for m05 = 0.5416
- Task 2: Selected m02 with measured QED = 0.6046