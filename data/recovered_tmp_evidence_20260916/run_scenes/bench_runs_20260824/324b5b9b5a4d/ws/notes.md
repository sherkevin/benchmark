# Notes

## Scaffold Analysis

Used `group_by_scaffold()` to identify 4 congeneric series:

1. **COc1ccc(cc1)C(=O)N** (methoxybenzamide): m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), **m05(C11)**
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrobenzamide): m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar acid): m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl): m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10)

## Task 1: Predict QED of m05

m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC, chain_len=11) has never been synthesized.
Measured all 5 available congeners in its series:

| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### QED Component Trends

| Component | m15(C2) | m24(C3) | m28(C4) | m17(C5) | m22(C6) | m05(C11) expectation |
|-----------|---------|---------|---------|---------|---------|----------------------|
| MW        | 0.3319  | 0.4062  | 0.4914  | 0.5853  | 0.6836  | Improves (~0.9)      |
| ALOGP     | 0.8736  | 0.9363  | 0.9768  | 0.9973  | 0.9973  | Drops (ALOGP=4.956)  |
| HBA       | 0.9305  | 0.9305  | 0.9305  | 0.9305  | 0.9305  | Same                 |
| HBD       | 0.9866  | 0.9866  | 0.9866  | 0.9866  | 0.9866  | Same                 |
| PSA       | 0.933   | 0.933   | 0.933   | 0.933   | 0.933   | Same                 |
| ROTB      | 0.9927  | 0.9697  | 0.8476  | 0.6885  | 0.5332  | Drops (ROTB=12)      |
| AROM      | 0.8273  | 0.8273  | 0.8273  | 0.8273  | 0.8273  | Same                 |
| ALERTS    | 0.8425  | 0.8425  | 0.6397  | 0.6397  | 0.6397  | Same                 |

### Polynomial Fits for m05 (chain_len=11)

| Fit | Points | Prediction | RMSE   |
|-----|--------|-----------|--------|
| **Quadratic (deg 2)** | **all 5 (2-6)** | **0.5416** | **0.012** |
| Linear (deg 1) | all 5 (2-6) | 0.6950 | 0.013 |
| Linear (deg 1) | chains 3-6 | 0.6508 | 0.009 |
| Linear (deg 1) | chains 4-6 | 0.7041 | 0.002 |
| Cubic (deg 3) | all 5 (2-6) | 2.1323 | invalid (>1) |
| Quadratic (deg 2) | chains 3-6 | 0.9378 | invalid (curves up) |

**Selected: Quadratic on all 5 points = 0.5416**
Rationale: Uses all data, captures non-linear trend, physically plausible (0-1 range), good RMSE.

## Task 2: Pick a Lead Molecule

Constraints: measured QED ≥ 0.6, PSA ≥ 40, NOT in m05's congeneric series.

### PSA by series
- Methoxybenzamide (m05's series): PSA ~38.33 < 40 → all excluded
- Nitrobenzamide: PSA = 72.24 ≥ 40 ✓
- Sugar acid: PSA = 110.02 ≥ 40 ✓
- Biphenyl: PSA = 29.1 < 40 → all excluded

### Measurements for Task 2

| mol_id | Series | PSA    | QED     | Qualifies? |
|--------|--------|--------|---------|------------|
| m01    | Sugar  | 110.02 | 0.3187  | ✗ QED<0.6 |
| m02    | Nitro  | 72.24  | 0.6046  | ✓ ALL THREE |
| m09    | Nitro  | 72.24  | 0.5826  | ✗ QED<0.6 |
| m12    | Nitro  | 72.24  | 0.4709  | ✗ QED<0.6 |

### Selected Lead: **m02**
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N — NOT m05's series ✓
- QED components: MW=0.50, ALOGP=0.92, HBA=0.98, HBD=0.99, PSA=0.93, ROTB=0.97, AROM=0.83, ALERTS=0.24

## Budget Usage
- list_pool: 3 pages (1, 2, 3) = 3 units
- measure_qed: 9 measurements (m15, m24, m28, m17, m22, m01, m02, m09, m12) = 9 units
- Total: 12/12 units used
