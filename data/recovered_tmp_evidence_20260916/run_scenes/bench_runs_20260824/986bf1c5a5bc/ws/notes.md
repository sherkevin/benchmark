# Notes: Medicinal Chemistry Analysis

## Job 1 — Predicting QED of m05

### Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxy-benzamide)
- N-alkyl chain length: 11 carbons
- Never synthesized → cannot measure directly

### Congeneric series identification
Used `describe_structure` to confirm all molecules sharing scaffold `COc1ccc(cc1)C(=O)N`:

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15    | 2         | COc1ccc(cc1)C(=O)NCC |
| m24    | 3         | COc1ccc(cc1)C(=O)NCCC |
| m28    | 4         | COc1ccc(cc1)C(=O)NCCCC |
| m17    | 5         | COc1ccc(cc1)C(=O)NCCCCC |
| m22    | 6         | COc1ccc(cc1)C(=O)NCCCCCC |
| m05    | 11        | (target, not measured) |

### QED measurements (congeners)
| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Observed trend
QED peaks at chain length 3 (0.7919) then steadily declines as chain length increases:
- Chain 2→3: +0.030 (increase)
- Chain 3→4: -0.040 (sharp drop)
- Chain 4→5: -0.002 (slight drop)
- Chain 5→6: -0.012 (moderate drop)

The overall shape is an inverted curve — rises then falls — consistent with increasing lipophilicity (alogp grows with chain length) eventually penalizing drug-likeness.

### Polynomial fit (degree 2)
Using all 5 measured points:
- Coefficients (low to high): 0.74932, 0.017024, -0.003264
- RMSE on training points: 0.01188
- Prediction at chain 11: **QED = 0.5416**

For comparison, a linear fit gives 0.695 at chain 11 (RMSE 0.01308). The quadratic is preferred because:
1. Lower RMSE
2. The data clearly shows curvature (rise then fall)
3. Task guidance: "Do not assume the relationship is a straight line"

The quadratic extrapolation predicts QED continues declining beyond chain 6 due to the -0.003264·x² term, reaching 0.5416 at chain 11. This makes chemical sense: very long N-alkyl chains drive logP too high for good drug-likeness.

### Prediction: m05 QED = 0.5416

---

## Job 2 — Backup Lead Compound

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check)
3. NOT in the same series as m05

### Screening strategy
1. Used `screen_candidates()` → 18 molecules pass PSA ≥ 40
2. None of the m05-series congeners (m15, m17, m22, m24, m28) pass PSA screen (all have PSA=38.33), so all 18 PSA-passing candidates are from other series
3. Selected 3 candidates from the nitro-benzamide series (`O=[N+]([O-])c1ccc(cc1)C(=O)N`) for QED measurement, prioritizing short-chain members with moderate logP:

| mol_id | chain | alogp | PSA   | QED (measured) | Pass? |
|--------|-------|-------|-------|----------------|-------|
| m02    | 3     | 1.735 | 72.24 | 0.6046         | ✓ ALL |
| m12    | 4     | 2.125 | 72.24 | 0.4709         | ✗ QED |
| m32    | 5     | 2.515 | 72.24 | 0.4683         | ✗ QED |

### Lead selection: m02
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Series: nitro-benzamide (NOT m05's methoxy-benzamide) ✓

---

## Budget usage
- 9 measurements for listing pool (9 pages × 1 each)
- 5 measurements for m05 congeners (m15, m24, m28, m17, m22)
- 3 measurements for lead candidates (m02, m12, m32)
- Total: 17/17 measurements used
