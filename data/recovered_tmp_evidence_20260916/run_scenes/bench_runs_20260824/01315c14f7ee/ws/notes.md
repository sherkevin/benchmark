# Notes

## Approach

### Scaffold Analysis (free)
Used `group_by_scaffold()` to identify 4 scaffold groups:
1. **Scaffold A** `COc1ccc(cc1)C(=O)N` — m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **Scaffold B** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **Scaffold C** `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **Scaffold D** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

### Task 1 — Predict QED of m05

Measured all 5 synthesizable members of m05's congeneric series:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fitted polynomial models to predict QED at chain_len=11:
- Degree 1 (all 5 pts): 0.695 (RMSE 0.013)
- Degree 2 (all 5 pts): **0.5416** (RMSE 0.012) ← selected
- Degree 3 (all 5 pts): 2.13 (absurd, overfitting)
- Degree 1 (chains 3–6 only): 0.6508
- Degree 2 (chains 3–6 only): 0.9378 (positive curvature, physically unlikely)

Selected degree-2 fit with all 5 points: peak near chain 2.6 then monotone decline — physically reasonable for increasing lipophilicity with longer chains.

**Predicted QED for m05: 0.5416**

### Task 2 — Find a lead molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's scaffold.

Scaffold D has PSA ≈ 29 (too low). Focused on Scaffolds B and C.

| mol_id | scaffold | PSA     | QED    | Qualifies? |
|--------|----------|---------|--------|------------|
| m01    | C        | 110.02  | 0.3187 | No (QED)   |
| m02    | B        | 72.24   | 0.6046 | **YES**    |
| m03    | C        | 110.02  | 0.3282 | No (QED)   |
| m09    | B        | ~72     | 0.5826 | No (QED)   |
| m12    | B        | ~72     | 0.4709 | No (QED)   |
| m14    | C        | 110.02  | 0.3237 | No (QED)   |
| m32    | B        | ~72     | 0.4683 | No (QED)   |
| m33    | B        | ~72     | 0.4591 | No (QED)   |

**Lead: m02** — QED 0.6046, PSA 72.24, Scaffold B (different from m05).

## Budget Usage
- 1 page listing = 1 unit
- 5 measurements for m05 series = 5 units
- 6 measurements for Task 2 candidates (batch 1) = 6 units
- 2 measurements for Task 2 candidates (batch 2) = 2 units
- **Total: 14 / 17 units used**
