# Notes

## Scaffold Grouping
Used `group_by_scaffold()` (free). Found 4 scaffold groups:

1. **Scaffold "COc1ccc(cc1)C(=O)N"** (m05's series): m15(chain 2), m24(chain 3), m28(chain 4), m17(chain 5), m22(chain 6), **m05(chain 11)**
2. **Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N"**: m09, m02, m12, m32, m33, m29, m31, m06, m10
3. **Scaffold "OCC(O)C(O)C(O)C(=O)N"** (sugar-like): m14, m27, m01, m19, m03, m21, m18, m07, m25
4. **Scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N"** (biphenyl): m13, m11, m04, m30, m23, m20, m26, m08, m16

## Task 1: Predict QED of m05 (chain_len=11)

Measured all 5 available members of m05's congeneric series:

| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### QED Components Analysis
Key changing components across the series:
- **MW**: 0.3319 → 0.6836 (increasing, entering favorable range)
- **ALOGP**: 0.8736 → 0.9973 (increasing toward optimal)
- **ROTB**: 0.9927 → 0.5332 (decreasing — main penalty driver)
- **ALERTS**: 0.8425 → 0.6397 (drops after chain 3)
- HBA, HBD, PSA, AROM stay constant

m05 has alogp=4.956 and rotb=12, so ROTB and ALOGP desirability will drop further.

### Polynomial Extrapolation
- **Degree 2, all 5 points**: QED = 0.5416 (RMSE = 0.01188) ← **best estimate**
- Degree 1, all 5 points: QED = 0.695 (RMSE = 0.01308) — too optimistic
- Degree 3, all 5 points: QED = 2.13 — nonsensical (overfitting)
- Degree 2, chains 4-6 only: QED = 0.5257 — consistent with main estimate

The quadratic model captures the accelerating decline in QED as chain lengthens (compounding ROTB and eventually ALOGP penalties). **Predicted m05 QED ≈ 0.5416**.

## Task 2: Find Lead Molecule (QED ≥ 0.6, PSA ≥ 40, not in m05's series)

### PSA filtering from catalogue pages
- Scaffold 1 (same as m05): PSA ~38 → excluded anyway (same series)
- Scaffold 2: PSA = 72.24 ✓
- Scaffold 3: PSA = 110.02 ✓ (but very negative alogp likely hurts QED)
- Scaffold 4: PSA = 29.1 ✗ (fails PSA constraint)

### QED Measurements for Candidates
| mol_id | scaffold | QED     | PSA    | Qualifies? |
|--------|----------|---------|--------|------------|
| m09    | 2        | 0.5826  | 72.24  | No (QED < 0.6) |
| m02    | 2        | 0.6046  | 72.24  | **Yes** ✓  |
| m12    | 2        | 0.4709  | 72.24  | No (QED < 0.6) |
| m13    | 4        | 0.8540  | 29.1   | No (PSA < 40) |

### Selected Lead: **m02**
- QED measured: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Scaffold 2 ≠ Scaffold 1 (different congeneric series from m05 ✓)

## Budget Usage
- Pages listed: 3 (pages 1-3)
- QED measurements: 9 (m15, m24, m28, m17, m22, m09, m02, m12, m13)
- Total: 12/12
